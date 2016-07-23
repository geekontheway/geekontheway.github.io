---
layout: post
title: "HTTP TCP UDP SOCKET"
date: 2011-12-15 19:40
comments: true
categories: [HTTP,TCP,UDP,SOCKET]
---

![image](http://hbimg.b0.upaiyun.com/896c31867495ba6b77820de84d3db64fadaa39d132523-JnHkwZ_fw658)

从上图可以看到，TCP/IP是个协议组，可分为三个层次：网络层、传输层和应用层。

在网络层有IP协议、ICMP协议、ARP协议、RARP协议和BOOTP协议。

在传输层中有TCP协议与UDP协议。

在应用层有FTP、HTTP、TELNET、SMTP、DNS等协议。

默认情况下HTTP使用TCP，但是也可以基于以后存在的其他可靠传输协议。由于UDP无法提供可靠传输和上HTTP的无记忆应答方式，所以不会使用UDP。

下图也是描述这个关系的。

![image](http://hbimg.b0.upaiyun.com/8b26ab0adec3671979d2240d1a7594b0d2ed5ed22ce95-bHuhEx_fw658)

Socket是应用层与TCP/IP协议族通信的中间软件抽象层，它是一组接口。

在设计模式中，Socket其实就是一个门面模式，它把复杂的TCP/IP协议族隐藏在Socket接口后面，对用户来说，一组简单的接口就是全部，让Socket去组织数据，以符合指定的协议。

TCP通信需要服务器端侦听listen、接收客户端连接请求accept，等待客户端connect建立连接后才能进行数据包的收发(recv/send)工作。

而UDP则服务器和客户端的概念不明显，服务器端即接收端需要绑定端口，等待客户端的数据的到来。后续便可以进行数据的收发(recvfrom/sendto)工作。

![image](http://hbimg.b0.upaiyun.com/9296f721008dfeafb2abc38e07867cccb5ba127d1f9c7-dHjria_fw658)