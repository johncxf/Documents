## Docker

#### 常用指令

| 指令                                        | 说明                                               |
| :------------------------------------------ | :------------------------------------------------- |
| docker -v                                   | 查看docker的版本                                   |
| docker history                              | 查看镜像历史                                       |
| docker info                                 | 查看docker的详细信息                               |
| docker images                               | 查看当前所有的镜像                                 |
| docker ps -a                                | 查看当前启动的、停止的所有容器                     |
| docker build -t xx/ownname <Dockerfile路径> | 构建自己的镜像xx/ownname是自己的托管代码路径       |
| docker exec it containername /bin/bash      | 进入容器内部查看或操作                             |
| docker inspect container-name               | 查看某容器的详细配置信息                           |
| docker load < /home/save.tar                | 加载镜像                                           |
| docker login <daocloud.io>                  | 登录某镜像平台，默认是hub.docker.com，需要注册账户 |
| docker logs -f <容器名或ID>                 | 查看容器日志                                       |
| docker pause <docker name or id>            | 暂停某一容器所有进程                               |
| docker pull <镜像名:tag                     | 从远程拉取镜像，不带tag视为latest                  |
| docker push name:tag                        | 将镜像推送到远程仓库                               |
| docker rm ${docker ps -a -q}                | 删除所有容器                                       |
| docker rm -f <容器名或ID>                   | 删除某容器                                         |
| docker rmi -f <镜像名或ID>                  | 强制删除某镜像                                     |
| docker run -it -d -p --name                 | 运行一个容器,命名、设置端口映射、后台运行等        |
| docker save busy-box>/home/save.tar         | 保存镜像                                           |
| docker start container-name                 | 启动容器                                           |
| docker stop container-name                  | 停止容器                                           |
| docker tag                                  | 标记本地镜像                                       |
| docker unpause <docker name or id>          | 回复某一容器的所有进程                             |