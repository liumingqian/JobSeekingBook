# Http

HTTP属于应用层协议，在传输层使用TCP协议，在网络层使用IP协议。

HTTP是一个无状态的面向连接的协议，【无状态】指的是协议对于事务处理没有记忆能力，服务器不知道客户端是什么状态。

### 长连接和短连接

 　HTTP的长连接和短连接本质上是TCP长连接和短连接**。**

**在HTTP/1.0中，默认使用的是短连接**。也就是说，浏览器和服务器每进行一次HTTP操作，就建立一次连接，但任务结束就中断连接，双方关闭连接时都不通知对方。 

从 **HTTP/1.1起，默认使用长连接**，用以保持连接特性，当一个网页打开完成后，客户端和服务器之间用于传输HTTP数据的 TCP连接不会关闭\(socket 结构不回收），如果客户端再次访问这个服务器上的网页，会继续使用这一条已经建立的连接。连接有时限。

### HTTP请求

HTTP协议是建立在请求/响应模型上的。首先由客户建立一条与服务器的TCP链接，并发送一个请求到服务器，请求中包含请求方法、URI、协议版本以及相关的MIME样式的消息。服务器响应一个状态行，包含消息的协议版本、一个成功和失败码以及相关的MIME式样的消息。



#### 浏览器输入一个地址回车之后都发生了啥？

1. 浏览器检查地址格式合法性
2. dns服务通过域名找出其ip地址
   1. 检查浏览器缓存
   2. 检查系统缓存\(hosts\)
   3. 请求检查路由器缓存
   4. 请求DNS域名系统根服务器
3. 通过TCP或UDP协议传输数据

[https://mp.weixin.qq.com/s?\_\_biz=Mzg2NzA4MTkxNQ==&mid=2247485625&idx=1&sn=ea3010c3c9fb167b30637c271f7a4a6a&chksm=ce40436df937ca7b2f5870a50fd339972bdfced68a328321f83fa5947bfa0ec9fe34c438ad27&scene=0&xtrack=1&key=95b17fddd06e963cf722b21924825df79789535339d8697c0323f4d77218fd69e607cfe033ab4ac51eeb2033a21cc8885bb323999bc6bd37b9a91c71686da4975de63134c4801ada944d4591c38fe3fd&ascene=1&uin=MTIyODM5ODk2NA%3D%3D&devicetype=Windows+10&version=62060833&lang=zh\_CN&pass\_ticket=bU9wDyDukiYw%2FWXImxaqS0011sxjYpOnB8TDQBM3XYyYuMigTjuu1vP32pCsmSkp](https://mp.weixin.qq.com/s?__biz=Mzg2NzA4MTkxNQ==&mid=2247485625&idx=1&sn=ea3010c3c9fb167b30637c271f7a4a6a&chksm=ce40436df937ca7b2f5870a50fd339972bdfced68a328321f83fa5947bfa0ec9fe34c438ad27&scene=0&xtrack=1&key=95b17fddd06e963cf722b21924825df79789535339d8697c0323f4d77218fd69e607cfe033ab4ac51eeb2033a21cc8885bb323999bc6bd37b9a91c71686da4975de63134c4801ada944d4591c38fe3fd&ascene=1&uin=MTIyODM5ODk2NA%3D%3D&devicetype=Windows+10&version=62060833&lang=zh_CN&pass_ticket=bU9wDyDukiYw%2FWXImxaqS0011sxjYpOnB8TDQBM3XYyYuMigTjuu1vP32pCsmSkp)

