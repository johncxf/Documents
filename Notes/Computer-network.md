# 计算机网络

## 知识点

#### TCP 的三次握手和四次挥手

##### 什么是三次握手、四次挥手？

TCP 是一种面向连接的单播协议，在发送数据前，通信双方必须在彼此间建立一条连接。所谓的“连接”，其实是客户端和服务器的内存里保存的一份关于对方的信息，如 ip 地址、端口号等。

TCP 可以看成是一种字节流，它会处理 IP 层或以下的层的丢包、重复以及错误问题。在连接的建立过程中，双方需要交换一些连接的参数。这些参数可以放在TCP头部。

TCP 提供了一种可靠、面向连接、字节流、传输层的服务，采用三次握手建立一个连接。采用四次挥手来关闭一个连接。

- ACK：确认、使得确认信号有效
- SYN：用于初始化连接的序列号
- FIN：该报文的发送方已经结束向对方发送数据

##### 三次握手

- 第一次握手：客户端发送syn包(syn=x)到服务器，并进入SYN_SEND状态，等待服务器确认；
- 第二次握手：服务器收到syn包，必须确认客户的SYN（ack=x+1），同时自己也发送一个SYN包（syn=y），即SYN+ACK包，此时服务器进入SYN_RECV状态；
- 第三次握手：客户端收到服务器的 SYN＋ACK 包，向服务器发送确认包 ACK(ack=y+1)，此包发送完毕，客户端和服务器进入ESTABLISHED状态，完成三次握手。

握手过程中传送的**包里不包含数据**，三次握手完毕后，客户端与服务器才正式开始传送数据。理想状态下，TCP连接一旦建立，在通信双方中的任何一方主动关闭连接之前，TCP 连接都将被一直保持下去。

##### 四次挥手

与建立连接的“三次握手”类似，断开一个TCP连接则需要“四次握手”。

- 第一次挥手：主动关闭方发送一个FIN，用来关闭主动方到被动关闭方的数据传送，也就是主动关闭方告诉被动关闭方：我已经不 会再给你发数据了(当然，在fin包之前发送出去的数据，如果没有收到对应的ack确认报文，主动关闭方依然会重发这些数据)，但是，此时主动关闭方还可以接受数据。
- 第二次挥手：被动关闭方收到FIN包后，发送一个ACK给对方，确认序号为收到序号+1（与SYN相同，一个FIN占用一个序号）。
- 第三次挥手：被动关闭方发送一个FIN，用来关闭被动关闭方到主动关闭方的数据传送，也就是告诉主动关闭方，我的数据也发送完了，不会再给你发数据了。
- 第四次挥手：主动关闭方收到FIN后，发送一个ACK给被动关闭方，确认序号为收到序号+1，至此，完成四次挥手。

**总结：**

> **三次握手：**客户端发送请求等待服务器端确认；服务器端收到请求同时发送请求给客户端，等待客户端回应；客户端收到服务器端的请求，向服务器端发送确认包，发送完毕，客户端和服务器端进入ESTABLISHED状态

##### 为什么会采用三次握手，若采用二次握手可以吗？

TCP的三次握手过程：主机A向B发送连接请求；主机B对收到的主机A的报文段进行确认；主机A再次对主机B的确认进行确认。

采用三次握手是为了防止失效的连接请求报文段突然又传送到主机B，因而产生错误。失效的连接请求报文段是指：主机A发出的连接请求没有收到主机B的确认，于是经过一段时间后，主机A又重新向主机B发送连接请求，且建立成功，顺序完成数据传输。考虑这样一种特殊情况，主机A第一次发送的连接请求并没有丢失，而是因为网络节点导致延迟达到主机B，主机B以为是主机A又发起的新连接，于是主机B同意连接，并向主机A发回确认，但是此时主机A根本不会理会，主机B就一直在等待主机A发送数据，导致主机B的资源浪费。
采用两次握手不行，原因就是上面说的失效的连接请求的特殊情况。

总结：

- 第一次握手，服务端可知客户端的发送能力、服务端的接受能力OK
- 第二次握手，客户端可知服务端和客户端的发送能力、接受能力都OK
- 第三次握手，服务端可知服务端和客户端的发送能力、接受能力都OK

##### 为什么需要四次挥手？

TCP连接是双向传输的对等的模式，就是说双方都可以同时向对方发送或接收数据。

当有一方要关闭连接时，会发送指令告知对方，我要关闭连接了。这时对方会回一个ACK，此时一个方向的连接关闭。

但是另一个方向仍然可以继续传输数据，等到发送完了所有的数据后，会发送一个FIN段来关闭此方向上的连接。

因此，另一个方向也需要用同样的方式关闭连接。

#### DNS域名系统，简单描述其工作原理。

当DNS客户机需要在程序中使用名称时，它会查询DNS服务器来解析该名称。客户机发送的每条查询信息包括三条信息：包括：指定的DNS域名，指定的查询类型，DNS域名的指定类别。基于**UDP服务**，**端口53**. 该应用一般不直接为用户使用，而是为其他应用服务，如HTTP，SMTP等在其中需要完成主机名到IP地址的转换。

#### http和https的区别

- HTTP：超文本传输协议、明文、80端口、不安全
- HTTPS：ca证书、ssl加密传输、443端口、安全、HTTPS连接缓存不如HTTP高效

#### HTTP的基本优化

带宽、延迟（浏览器阻塞、DNS查询、建立连接）

#### HTTP1.0和HTTP1.1的一些区别

缓存处理、带宽优化及网络连接的使用、错误通知的管理、host头处理、长连接

#### SPDY：HTTP1.x的优化

降低延迟、请求优先级、header压缩、基于https的加密协议传输、服务器推送

#### HTTP2.0和HTTP1.X相比

新的二进制格式、多路复用、header压缩、服务器推送

#### http请求方法有哪些？

- GET
- POST
- HEAD
- PUT
- DELETE
- ...

#### GET和POST的区别

- GET参数通过URL传递，POST放在Request body中。
- GET在浏览器回退时是无害的，而POST会再次提交请求。
- GET产生的URL地址可以被Bookmark，而POST不可以。
- GET请求会被浏览器主动cache，而POST不会，除非手动设置。
- GET请求只能进行url编码，而POST支持多种编码方式。
- GET请求参数会被完整保留在浏览器历史记录里，而POST中的参数不会被保留。
- GET请求在URL中传送的参数是有长度限制的，而POST没有。
- 对参数的数据类型，GET只接受ASCII字符，而POST没有限制。
- GET比POST更不安全，因为参数直接暴露在URL上，所以不能用来传递敏感信息。
- GET产生一个TCP数据包；POST产生两个TCP数据包。

#### XSS（跨站脚本）攻击

发生在目标用户的浏览器层面上的，当渲染DOM树的过程成发生了不在预期内执行的JS代码时，就发生了XSS攻击。大多数XSS攻击的主要方式是嵌入一段远程或者第三方域上的JS代码

- 一旦在DOM解析过程成出现不在预期内的改变（JS代码执行或样式大量变化时），就可能发生XSS攻击
- XSS分为反射型XSS，存储型XSS和DOM XSS
- 反射型XSS是在将XSS代码放在URL中，将参数提交到服务器。服务器解析后响应，在响应结果中存在XSS代码，最终通过浏览器解析执行。
- 存储型XSS是将XSS代码存储到服务端（数据库、内存、文件系统等），在下次请求同一个页面时就不需要带上XSS代码了，而是从服务器读取。
- DOM XSS的发生主要是在JS中使用eval造成的，所以应当避免使用eval语句。
- XSS危害有盗取用户cookie，通过JS或CSS改变样式，DDos造成正常用户无法得到服务器响应。
- XSS代码的预防主要通过对数据解码，再过滤掉危险标签、属性和事件等。

### HTTP

#### OSI七层、TCP/IP五层协议

**OSI** （Open System Interconnect），开放式系统互联，它是一个国际标准化组织制定的一个用于计算机或通信系统间互联的标准体系。

OSI 七层协议是一种国际规范，将网络协议分成了 7 层，分别是：物理层、数据链路层、网络层、传输层、会话层、表示层、应用层。

TCP/IP 四层协议是围绕 TCP/IP 展开的一系列通信协议，是参考 OSI 七层协议进行的分层

TCP/IP 五层协议是为了方便学习计算机网络原理而采用的，综合了OSI七层模型和TCP/IP的四层模型而得到的五层模型

OSI七层协议和 TCP/IP 四层、五层协议结构对应关系如下图所示：

![1557064350664](../Image/oldimg/1557064350664.png)

- 应用层
  - 作用：为操作系统或网络应用程序提供访问网络服务的接口
  - 协议：FTP、DNS、Telnet、SMTP、HTTP、WWW、NFS 等
  - 数据单元：APDU
- 表示层
  - 作用：提供格式化的表示和转换数据服务，数据的翻译、加密和压缩
  - 协议：JPEG、MPEG、ASII
  - 数据单元：PPDU
- 会话层
  - 作用：建立、管理和终止会话
  - 协议：NFS、SQL、NETBIOS、RPC
  - 数据单元：SPDU
- 传输层
  - 作用：提供端到端的可靠报文传递和错误恢复
  - 协议：TCP、UDP、SPX
  - 数据单元：段 Segment
- 网络层
  - 作用：负责数据包从源到宿的传递和网际互连
  - 协议：IP、ICMP、IGMP、ARP、RARP、OSPF、IPX、RIP、IGRP、 （路由器）
  - 数据单元：数据包 PackeT
- 数据链路层
  - 作用：将比特组装成帧和点到点的传递，作用包括：物理地址寻址、数据的成帧、流量控制、数据的检错、重发等
  - 协议：PPP、FR、HDLC、VLAN、MAC （网桥，交换机）
  - 数据单元：帧 Frame
- 物理层
  - 作用：通过媒介传输比特，确定机械及电气规范
  - 协议：RJ45、CLOCK、IEEE802.3 （中继器，集线器）
  - 数据单元：比特 Bit