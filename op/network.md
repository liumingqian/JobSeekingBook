# 网络

#### ref

[Rpc服务和HTTP服务对比](https://blog.csdn.net/wangyunpeng0319/article/details/78651998)

[如何实现一个简单的rpc](https://www.jianshu.com/p/5b90a4e70783)

### OSI网络七层模型 <a id="OSI&#x7F51;&#x7EDC;&#x4E03;&#x5C42;&#x6A21;&#x578B;"></a>

（前三层基本可以合并）

* 第一层：应用层。定义了用于在网络中进行通信和传输数据的接口；
* 第二层：表示层。定义不同的系统中数据的传输格式，编码和解码规范等；
* 第三层：会话层。管理用户的会话，控制用户间逻辑连接的建立和中断；
* 第四层：传输层。管理着网络中的端到端的数据传输；
* 第五层：网络层。定义网络设备间如何传输数据；
* 第六层：链路层。将上面的网络层的数据包封装成数据帧，便于物理层传输；
* 第七层：物理层。这一层主要就是传输这些二进制数据。

#### C/S架构

#### TCP/IP协议

一种传输层协议

UDP

TCP和UDP的区别

TCP和UDP的应用场景

time\_wait状态

#### HTTP协议

短链接

#### RPC\(远程过程调用）

是基于TCP/IP协议的一种服务。



RPC要解决的两个问题：

1. 解决分布式系统中，服务之间的调用问题。
2. 远程调用时，要能够像本地调用一样方便，让调用者感知不到远程调用的逻辑。

![&#x672C;&#x5730;&#x8FC7;&#x7A0B;&#x8C03;&#x7528;](../.gitbook/assets/image%20%285%29.png)

![RPC](../.gitbook/assets/image%20%2814%29.png)

一个完整的RPC流程，可以用下面这张图来描述：  


![](//upload-images.jianshu.io/upload_images/7143349-a9db3c3c85194c6e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/263/format/webp)

其中左边的Client，对应的就是前面的Service A，而右边的Server，对应的则是Service B。  
 下面一步一步详细解释一下。

1. Service A的应用层代码中，调用了Calculator的add方法；
2. 这个Calculator实现类，内部并不是直接实现计算器的加减乘除逻辑，而是通过远程调用Service B的RPC接口，来获取运算结果，因此称之为**Stub**；
3. Stub怎么和Service B建立远程通讯呢？这时候就要用到**远程通讯工具**了，也就是图中的**Run-time Library**，这个工具将帮你实现远程通讯的功能，比如Java的**Socket**，也可以用基于Http协议的**HttpClient**，或者其他通讯工具类，都可以，**RPC并没有规定说你要用何种协议进行通讯**；
4. Stub通过调用通讯工具提供的方法，和Service B建立起了通讯，然后将请求数据发给Service B。需要注意的是，由于底层的网络通讯是基于**二进制格式**的，因此这里Stub传给通讯工具类的数据也必须是二进制，比如calculator.add\(1,2\)，你必须把参数值1和2放到一个Request对象里头（这个Request对象当然不只这些信息，还包括要调用哪个服务的哪个RPC接口等其他信息），然后**序列化**为二进制，再传给通讯工具类，这一点也将在下面的代码实现中体现；
5. 二进制的数据传到Service B这一边了，Service B当然也有自己的通讯工具，通过这个通讯工具接收二进制的请求；
6. 既然数据是二进制的，那么自然要进行**反序列化**了，将二进制的数据反序列化为请求对象，然后将这个请求对象交给Service B的Stub处理；
7. service B的stub解析请求对象，知道调用方要调的是哪个RPC接口，传进来的参数又是什么，然后再把这些参数传给对应的RPC接口，也就是Calculator的实际实现类去执行。很明显，如果是Java，那这里肯定用到了**反射**。
8. RPC接口执行完毕，返回执行结果，Service B以同样的流程把数据发给Service A，Service A反序列化执行结果 ，将结果返回给Application。

浏览器输入一个地址回车之后都发生了啥？

