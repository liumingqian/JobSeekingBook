# TCP/IP协议族

## TCP/IP协议族

一个传输层协议族，包括IP协议、IMCP协议、TCP协议等。TCP/IP指的是TCP和IP协同工作，TCP负责应用软件（如浏览器）和网络软件间的通信，IP是网际协议，负责计算机间的通信，主要解决路由和寻址问题。

* 网络层：IP协议、ICMP协议
* 应用层：http、ftp、Telnet、SMTP、DNS

![](../../.gitbook/assets/image%20%2834%29.png)

### IP协议

是计算机间的无连接的通信协议。

### ICMP协议

## TCP协议

TCP用于应用程序之间的全双工通信。一个TCP连接由一个四元组唯一标识：&lt;source ip/port,target ip/port&gt;，以四元组为key，查找hash表中对应socket的结构指针，找到对应socket的接收缓冲区，将载荷数据拷贝进去。

### TCP的应用场景

对网络通信质量有要求时，整个数据要准确无误的传递给对方

* FTP
* HTTP/HTTPS
* SMTP（邮件传输协议）
* QQ文件传输
* QQ登录后会有一个TCP连接来保持在线状态

### 三次握手建立连接

三次握手后两方的初始序列号达成了统一，需要三次握手而不是两次可以避免网络拥堵的时候客户端超时重发，导致有多个连接请求抵达服务端，无法得到正确的初始序列号。

* 第一次：客户端发送请求，请求报文中包含同步标志位和初始序列号seq=x
* 第二次：服务器收到报文后同意链接，发出确认报文，序列号是x+1，并初始化一个自己的序列号seq=y
* 第三次：客户端收到确认后再次向服务器给出确认，发送序列号x+1，y+1，客户端进入建立连接的状态，服务器收到确认后也进入建立连接的状态

![](../../.gitbook/assets/image%20%2810%29.png)

### 四次挥手释放连接

Tcp连接是双向的，前两次挥手用于断开一个方向的连接，后两次用于断开另一方向的连接

* 第一次：A觉得数据发送完了，可以断开连接，向B发送连接释放请求，并进入FIN-WAIT-1状态
* 第二次：B收到后通知相应的应用程序，进入CLOSE-WAIT状态，并向A发送连接释放的确认。A收到应答后会进入FIN-WAIT-2状态，等待B发送连接释放请求，此时A到B的连接已经释放，A不会再向B发送数据，B不会接收A的数据，但B仍可向A发送数据
* 第三次：B向A发送完所有数据后向A发送连接释放请求，并进入LAST-ACK状态
* 第四次：A收到释放请求后向B发送确认应答，此时A进入TIME-WAIT状态，该状态持续2MSL时间，如果该时间段内B没有重发请求，A就进入CLOSED状态，撤销TCB。B收到A的确认应答后也进入CLOSED状态，撤销TCB。
  * TIME-WAIT状态是为了保证B能收到A的应答，如果A发送的确认应答丢失了，B等待超时后就会重新发送释放请求，如果A直接CLOSED的话就不会做出任何响应，导致B永远无法正常关闭

![](../../.gitbook/assets/image%20%2875%29.png)

### TCP协议的可靠性机制

TCP的可靠性体现在传输数据之前，三次握手建立连接（四次挥手断开连接），并且在数据传递时，有确认，窗口，重传，拥塞控制机制，数据传完之后断开连接来节省系统资源。

#### ACK延迟机制

服务器收到数据包后不马上发送ACK，而是等一小段时间，按最后一个正确的数据发送ACK，减少发送ACK次数

#### 滑动窗口

TCP通过滑动窗口来进行流量控制。在发送端发送数据的速度很快而接收端接收速度却很慢的情况下，为了保证数据不丢失，需要进行流量控制协调好通信双方的工作节奏。

![](../../.gitbook/assets/image%20%2879%29.png)

#### socket

socket是对TCP/IP协议的封装，它的出现只是使得程序员更方便地使用TCP/IP协议栈而已。socket本身并不是协议，它是应用层与TCP/IP协议族通信的中间软件抽象层，是一组调用接口（TCP/IP网络的API函数）

#### 

