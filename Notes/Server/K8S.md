# K8S

## 概念

K8S是 Kubernetes 的简称，是负责自动化运维管理多个Docker程序的集群（可以自动完成服务的部署、更新、卸载和扩容、缩容）。

K8S是属于主从设备模型（Master-Slave架构）

**Master Node 包含组件：**

- **API Server（K8S的请求入口服务）**：API Server负责接收K8S所有请求（来自UI界面或者CLI命令行工具），然后，API Server根据用户的具体请求，去通知其他组件干活。API Server实际可以部署多个实例，以增大请求吞吐。
- **Scheduler（K8S中的调度服务0**：当用户要部署服务时，Scheduler会选择最合适的Worker Node（服务器）来部署。Scheduler也可以部署多个实例，以增大处理能力。
- **Controller Manager（K8S的控制服务）**：Controller Manager本身就是总称，实际上有很多具体的Controller，如：Node Controller、Service Controller、Volume Controller等，分别负责不同K8S对象。举个例子，比如用户要求A服务部署2个副本，那么当其中一个服务挂了的时候，Controller会马上调整，让Scheduler再选择一个Worker Node重新部署服务。
- **etcd（K8S的存储服务）**：etcd存储了K8S的关键配置和用户配置，K8S中仅API Server才具备读写权限，其他组件必须通过API Server的接口才能读写数据。

**Slave Node 包含组件**：

- **Kubelet（Worker Node的监视器，以及与Master Node的通讯器）**：Kubelet是Master Node安插在Worker Node上的“眼线”，它会定期向Master Node汇报自己Node上运行的服务的状态，并接受来自Master Node的命令，并执行。
- **Kube-Proxy（K8S的网络代理）**：Kube-Proxy负责Node在K8S的网络通讯、以及对外部网络流量的负载均衡。
- **Container Runtime（Worker Node的运行环境）**：即安装了容器化所需的软件环境确保容器化程序能够跑起来，比如Docker Engine。大白话就是帮忙装好了Docker运行环境。
- **Logging Layer（K8S的监控状态收集器）**：Logging Layer负责采集Node上所有服务的CPU、内存、磁盘、网络等监控项信息。
- **Add-Ons（K8S管理运维Worker Node的插件组件）**：有些文章认为Worker Node只有三大组件，不包含Add-On，但笔者认为K8S系统提供了Add-On机制，让用户可以扩展更多定制化功能，是很不错的亮点。

**K8S的Master Node具备：请求入口管理（API Server），Worker Node调度（Scheduler），监控和自动调节（Controller Manager），以及存储功能（etcd）；而K8S的Worker Node具备：状态和监控收集（Kubelet），网络和负载均衡（Kube-Proxy）、保障容器化运行环境（Container Runtime）、以及定制化功能（Add-Ons）**

## POD

**Pod**是可以在 Kubernetes 中创建和管理的、最小的可部署的计算单元。

Pod可以被理解成一群可以共享网络、存储和计算资源的容器化服务的集合。在同一个Pod里的几个Docker服务/程序，好像被部署在同一台机器上，可以通过localhost互相访问，并且可以共用Pod里的存储资源。

同一个Pod之间的Container可以通过localhost互相访问，并且可以挂载Pod内所有的数据卷；但是不同的Pod之间的Container不能用localhost访问，也不能挂载其他Pod的数据卷。

