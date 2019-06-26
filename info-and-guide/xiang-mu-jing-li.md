# 项目经历

**数学库更换**

从核心类向外一圈一圈改，先把类里面的都改掉然后改接口，这样中间有很多可编译版本。CDVector用一个union保存struct{x,y,z}struct{X,Y,Z}、T mfecv\[3\],以满足多种数据需求，重载了各种运算符。

#### 多线程编程

TiledViewport：：SharedMemoryCmd 用于向无人机主控发送命令的共享内存类

#### 线程池

地形模块为所有网络和本地db的地形块数据异步请求创建了一个基类job\_worker。job\_worker管理一个线程池。由各个CacheManager请求时向其中添加任务，用boost::bind绑定了process函数进行请求或上采样，下采样。

同时有一个job\_queue类负责存储这些任务。job\_queue用锁保证线程安全性，并封装了一些sortPush等功能。外部增加任务时尝试调用job\_queue的try\_push函数。

#### viwo插件模式

clientCore：pluginManager：autoLoadPlugins，根据配置文件的路径读取到插件名，然后再根据插件名到xml目录下找对应的插件的xml文件。对于一个插件模块，InvokePluginInit函数通过调用GetProcAddress去找模块中叫Init的函数，完成dll的初始化。

## 模型处理

### 模型序列化

### 模型错误修正

#### 调试技巧

* opengl没有单位，但三维软件导出fbx有单位，在导出时会根据软件系统单位和要导出的单位，对模型数据添加一个缩放因子，此时会导致部分网格被缩放的非常小。

### 模型动画

## Viwo地形模块

### 地形四叉树

![](../.gitbook/assets/image%20%281%29.png)

#### 层数确定和比例换算

鼠标滚轮缩放时调用MouseZRoll。调用到UpdateGlobalInfo函数，首先计算当前g\_screenUnit

* target：视线和地形交点
* distance:相机到target的距离
* int SimpleEditorConfig::SplitRes = TexRes &lt;&lt; 1; （TexRes=256, 超过splitRes就分裂\)
* int SimpleEditorConfig::MergeRes = TexRes &gt;&gt; 1;
* levelsize:每一个level单个地形块边长跨越的经纬度
* nearUnit：3层地形块的全球坐标下的边长/splitRes
* farUnit：17层地形块的全球坐标系下的边长/mergeRes

g\_screenUnit=（FrustumRight-FrustumLeft）/窗口宽度（像素）\*distance/近裁面距离

（FrustumRight-FrustumLeft）/窗口宽度（像素）可以理解为一个opengl单位（一般是一米）对应多少像素，这个值要乘上近裁面到视点所在位置的比例（聚焦在视点），因此上式可以求出一个单位为米/像素的值。

最后还需根据相机高度更新相机，如果相机高度很高，则需要等比例的缩放视锥。

### 地形编辑

#### 编辑数据同步

* 保存金字塔结构的数据：设置一个临时db，同步更新修改层的上层数据，异步更新其下层数据，保证实时交互效率；在保存状态下，真正将所有层数据存入与原引擎匹配的数据库。
* LRU数据管理：已编辑数据不能从LRU中换出，对相应Cache加锁
* 增加版本信息避免异步更新下层数据，请求父节点数据时获取到的不是最新的数据
* 注意边界问题以防止修改后出现T-junction

## 飞控项目

### AirSim是做什么的？

模拟器是无人机在真实世界里的视觉呈现，Airsim是飞控到渲染引擎的一个桥梁，支持了simpleFlight、px4、ardupilot等多种飞控库，接受用户输入的指令，经包装（mavlink）发送给飞控库，并接收解算结果，指导渲染引擎的渲染。Airsim用unreal自带的一些组件如WheeledVehicle、rotationMovementComponent等，提供了默认的无人机和无人车模型，使用户可以快速的即插即用的获得无人机无人车仿真体验。

### 为了应用Airsim做了哪些工作？

飞行控制器的主要工作是将期望状态作为输入，利用传感器数据估计实际状态，然后驱动电机使实际状态接近期望状态

* 为多人模式进行了适配
  * 原先AirSim没有用playerController，直接在Pawn的初始化里绑定了轴和key来接受输入。然后在把输入控制放到playerController里的过程中遇到了一些初始化顺序问题，通过重载postLogin等掌握关键时间节点的函数保证了初始化顺序正确
  * 多个ROS-Airsim端，通过多个端口连接到Unreal。在场景中放置playerStart后，在多玩家选项中设置玩家数量，unreal会在playerStart处根据gameMode中指定的默认playerController、pawn和HUD类创建player。controller和pawn都是首先在服务器上创建n个，如controller0，controller1，controller2，然后依次拷贝到每个客户端，每个客户端拥有的是controller0，controller0，controller0。重载GameMode的PostLogin函数获取Controller和Pawn在服务器创建并possess完成的时间节点。controller中保存simMode的指针，并在接受输入后通过simMode对vehicle进行控制，在PostLogin调用后，controller可以拿到pawn，然后委托SimHUD在客户端创建SimMode。
* Timer
  * 一个负责收发无人车无人机姿态信息和控制信息的类
  * 为无人车运动的速度设置一个小小的范围，每隔特定interval设置一次油门和刹车，检查无人车速度如果不在范围里就改变油门和刹车，使无人车速度稳定维持在期望值范围内。（非线性系统线性化？）

### 飞控为什么复杂？

飞行控制器的主要工作是将期望状态作为输入，利用传感器数据估计实际状态，然后驱动电机使实际状态接近期望状态。即，预测未来，修正当下。目前工业界经常用扩展卡尔曼滤波器（ekf）进行无人机多传感器融合。

* 涉及多个实体，运作机制复杂（模拟器、飞控模拟软件、地面站），多项配置（遥控器通道、遥控器校准、飞行模型选择、飞行模式选择，UDP网络配置）
* 资料少，从Airsim切入，但Airsim文档有些过期

### Unreal

#### server—client网络模型  

![](../.gitbook/assets/image%20%2854%29.png)

![](../.gitbook/assets/image%20%2868%29.png)

client去获取它没有权限获得的GameMode的时候只会得到空指针

* Gamemode：游戏规则，只存在在server
* GameState：在server和Client中间传递信息，从server复制到每个客户端的
* playerState:代表player的所有信息，也是拷贝给每个人的，这样每个client都知道所有玩家的信息。当切换地图或者断线重连的时候，playerState保存了之前的游戏状态

## 2D地图模块

### camera2D怎么工作?

工作流：

1. 3D相机更新位置
2. 计算rate：小地图宽度/相机所照到的实际宽度

   double rate = _\(PI \*_ g_coord.getShortAxis\(\) \* camera_-&gt;GetWindowHeight\(\)\) / \(40075453 \* _camera\_-&gt;GetElevation\(\) \*_ tan\(camera\_-&gt;GetFovy\(\) / 2 \* PI / 180\)\);

   1. 其中fovy是相机视角，默认为45度，角度\*pi/180是转弧度，40076km是赤道周长，shortAxis是地球半径，记为r，赤道周长记为c，公式可以转化为【\(pi\*r/c\)\*小地图高度 】/【相机高度\*tan（一半相机视角）】。
   2. 前半边中2\*pi\*r是圆周长，pi\*r是墨卡托下从南极到北极的距离，pi\*r/c是墨卡托坐标系下地图的长宽比，乘上小窗口height得到小窗口的理论宽度。后半边求得的是相机照到的实际宽度的一半。
   3. 数据源将地图分割成256\*256的小块

![](../.gitbook/assets/image%20%2818%29.png)

![](../.gitbook/assets/image%20%2831%29.png)

## **调试方法**

> 云风blog
>
> 我的理念中，一切调试方法都比不上 Code Review 。无论是自己写的代码，还是半途介入的别人的代码。第一要务就是要先理解程序的总体结构。分支的多少决定了代码的复杂度，在扫描代码的同时，大脑其实是在同时分析所有可能的情况，同时还能对不太重要的分支做剪枝。分析速度和能分析的宽度（复杂度）以及剪枝的正确性是需要反复训练才能拓展的。过于依赖交互式调试工具会影响这种训练，大脑受工具的影响，会更关心眼下的状态：目前运行到哪里了，（为了提高调试效率）下个断点设到哪里去，现在这组变量的值是什么…… 而不太关心：如果输入是另外一种情况，程序将怎么运行。因为工具已经把这些没有发生的过程剪掉了，等着你设计另一组输入下次再展示给你。  
> 中途介入的他人的项目需要运用你对相关领域的知识，和同类软件通常的设计模式，预设软件可能的模块划分方式。这个过程需要对领域的理解，不应过度陷入代码实现细节。一上手就开调试器先跑跑软件的大致运行流程是我不太推荐的方法。这样视野太狭窄了，花了不少时间只观察到了局部。

## 性能优化

* 用更好的库
* 用更好的算法
  * gltf
  * 四叉树分裂策略
  * 预存db
  * cache缓存（云block，地形块\)
* 用更好的数据结构
* 减少内存分配和复制
  * 静态分配效率优于动态分配
  * 内存碎片可能导致长时间运行后性能突然下降
  * 减少临时变量造成的冗余拷贝
* 技巧
  * 用位运算进行乘除，取模等操作

