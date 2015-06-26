---
layout: post
title: "UNIX_Network"
description: "UNIX Network Programming"
category: Network
tags: [Network]
---
{% include JB/setup %}

#Chapter 1 Introduction

##Chapter 1.7 OSI Model
![](http://i.imgur.com/xx3mk3n.png)

#Chpater 2 The Transport Layer: TCP, UDP, and SCTP

##Chapter 2.3 User Datagram Protocol (UDP)
The application writes a message to a UDP socket, which is then *encapsulated* in a UDP datagram, which is then further encapsulated as an IP datagram, which is then sent to its destination.

There is *no guarantee* that a UDP datagram will ever reach its final destination, that order will be preserved across the network, or that datagrams arrive only once.

We also say that UDP provides a *connectionless* service, as there need not be any long-term relationship between a UDP client and server.

##Chapter 2.6 TCP Connection Establishment and Termination

###Establishment
1. The server must be prepared to accept an incoming connection. This is normally done by calling `socket`, `bind`, and `listen` and is called a *passive open*.
2. The client issues an *active* open by calling `connect`. This causes the client TCP to send a "synchronize" (SYN) segment, which tells the server the client's initial sequence number for the data that the client will send on the connection. Normally, there is no data sent with the SYN; it just contains an IP header, a TCP header, and possible TCP options (which we will talk about shortly).**[First Handshake]**
3. The server must acknowledge (ACK) the client's SYN and the server must also send its own SYN containing the initial sequence number for the data that the server will send on the connection. The server sends its SYN and the ACK of the client's SYN in a single segment.**[Second Handshake]**
4. The client must acknowledge the server's SYN.**[Third Handshake]**
![](http://i.imgur.com/hHfcacz.png)

###Termination
1. One application calls `close` first, and we say that this end performs the *active close*. This end's TCP sends a FIN segment, which means it is finished sending data.
2. The other end that receives the FIN performs the *passive close*. The received FIN is acknowledged by TCP. The receipt of the FIN is also passed to the application as an end-of-file (after any data that may have already been queued for the application to receive), since the receipt of the FIN means the application will not receive any additional data on the connection.
3. Sometime later, the application that received the end-of-file will `close` its socket. This causes its TCP to send a FIN.
4. The TCP on the system that receives this final FIN (the end that did the active close) acknowledges the FIN.

![](http://i.imgur.com/3fYTVG4.png)

##Chapter 2.9 Port Number

###Socket Pair
The *socket pair* for a TCP connection is the four-tuple that defines the two endpoints of the connection: the local IP address, local port, foreign IP address, and foreign port.

##Chapter 4 Elementary TCP Sockets
![](http://i.imgur.com/uCBrH8E.png)

###Chapter 4.2 `socket` Function

> `socket`函数指定期望的通信协议类型。
	
<pre class = "brush:cpp">
	#include &lt;sys/socket.h&gt;
	int socket(int family, int type, int protocol);
	//若成功返回非负描述符，失败返回-1
</pre>

`socket`函数在成功时返回一个小的非负整数值，它与文件描述符相似，成为套接字描述符(socket descriptor)。为了得到这个套接字描述符，我们只是指定了协议族(IPv4, IPv6, Unix)和套接字类型(字节流，数据报，原始套接字)。并没有指定本地协议地址和远程协议地址。

###Chapter 4.3 `connect` Function

> TCP客户端用`connect`函数来建立与TCP服务器的连接。

<pre class = "brush:cpp">
	#include &lt;sys/socket.h&gt;
	int connect(int sockfd, const struct sockaddr *servaddr, socklen_t addrlen);
	//若成功则返回0，出错则返回-1
</pre>

客户端在调用`connect`之前不必调用`bind`函数，因为如果需要的话，内核会确定源IP地址，并选择一个临时端口作为源端口。

如果是TCP套接字，调用`connect`函数将激发**TCP三路握手**过程，并且尽在连接成功或出错时才返回。

##Chapter 4.4 `bind` Function

> `bind`函数把一个本地协议地址赋予一个套接字

<pre class = "brush:cpp">
	#include &lt;sys/socket.h&gt;
	int bind(int sockfd, const struct sockaddr *myaddr, socklen_t addrlen);
	//若成功则返回0，失败返回-1
</pre>