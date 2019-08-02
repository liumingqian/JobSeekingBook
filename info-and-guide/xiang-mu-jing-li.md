# 项目经历

**lambda表达式、函数指针、比较函数（std::greater\)、桥接模式写法、bind** 你在viwo中做了什么

**数学库更换**

从核心类向外一圈一圈改，先把类里面的都改掉然后改接口，这样中间有很多可编译版本。CDVector用一个union保存struct{x,y,z}struct{X,Y,Z}、T mfecv\[3\],以满足多种数据需求，重载了各种运算符。

#### 多线程编程

TiledViewport：：SharedMemoryCmd 用于向无人机主控发送命令的共享内存类

#### 线程池

地形模块为所有网络和本地db的地形块数据异步请求创建了一个基类job\_worker。job\_worker管理一个线程池。由各个CacheManager请求时向其中添加任务，用boost::bind绑定了process函数进行请求或上采样，下采样。

同时有一个job\_queue类负责存储这些任务。job\_queue用锁保证线程安全性，并封装了一些sortPush等功能。外部增加任务时尝试调用job\_queue的try\_push函数。

Tex、Dem、Mask、BoundingBox等数据

#### viwo插件模式

clientCore：pluginManager：autoLoadPlugins，根据配置文件的路径读取到插件名，然后再根据插件名到xml目录下找对应的插件的xml文件。对于一个插件模块，InvokePluginInit函数通过调用GetProcAddress去找模块中叫Init的函数，完成dll的初始化。

#### viwo加载效率优化

没有cpu端的剪裁，通过视锥剪裁，将模型打包合并来优化绘制效率

## 场景构建

用实验室自研的场景编辑器进行场景构建，修改模型错误，编辑场景物体的运动

### 模型序列化

#### Gltf

* 是啥？

khronos推出的，致力于使其成为3D界的JPEG那样的通用格式的一种格式。目前支持多种常用的三维软件通过插件直接读写gltf格式，比如Maya、3dmax、unity等等，assimp也支持了gltf格式的读写。

* 为啥要用？

1、glTF = The GL Transmission Format，使用gltf可以享受三维数据格式统一的好处，并避免各个三维软件间处理大量的导入/导出脚本有缩放问题，动画问题，纹理绑定问题，材质问题。甚至连opengl中的纹理平铺方法这类的属性都保存下来，保证效果一定是对的了。2、对GL的api非常友好，可以用glBufferData将每个缓冲区加载到GPU中，然后用glVertexAttribPointer解析每个访问器，以绑定到缓冲区中每个顶点元素的位置。3、使用pbr材质模型

* 为什么不合并bufferview和Accessor？

 如果一个bufferview里的数据是pos\|normal\|pos\|normal这样排布的话，一个bufferView可以对应多个accessor来正确解释数据

* 工作流

用libgltf解析gltf\(其实是json格式），然后根据bufferView和Accessor解析buffer，用boost库解析base64编码的字段。

对网格按material排序，按数据种类分别传入，并在读入position的时候计算包围盒（gltf给了minmax\)，对于索引数组要转uint

#### 遇到的问题

模型画不出来：没有设置单位和比例

网格破碎：obj的index从1开始，glDrawElement的参数是面片数\*3，assimp的mesh index都是从0开始

累加的index爆short

纹理是黑的：glTexStorage之后用glTexImage填充内存，应该用glSubTexImage2D

路径中文乱码：boost库把string转成wstring

### 模型渲染

![](../.gitbook/assets/image%20%281%29.png)

### 模型错误修正

在三维软件中查看数据，将fbx写成可读模式查看，用assimp的后处理参数修正、导出正确的单位和比例，相对路径设置。

调试：尝试画部分网格，glgetError，画包围盒

### 模型动画

点击生成行车路线，用三弯矩方程构造三次样条曲线。

动画基类定义时钟，动画播放状态（loop，once，back&forth等）

RotationSimpleAnimation定义角速度和旋转矩阵

TranslationSimpleAnimation定义起点和终点

#### 改进

instance绘制的逻辑不应该由程序员负责，应该内部收集instance节点并决定要不要进行instance绘制。

增强鲁棒性，增加报错信息和警告，提示缺少资源等问题。

规范化animation，使其可以从配置文件中读取。

兼容关键帧动画。

## Viwo

### 渲染

#### 阴影

光照为平行光，阴影由阴影贴图实现。SSAO，每个片元采样64个点。

## Viwo地形模块

公司共享盘上有viwo和googleearth的比较！

#### viwo功能

编辑：高精度地形纹理编辑、河流植被编辑、建筑放置。

可视化：场景漫游、大规模场景的实时绘制。环境仿真（不同季节不同时段）、气象仿真、海洋仿真、粒子特效、阴影

每层地形块x的坐标范围为0-2^\(level+1\)-1,y的坐标范围为0-2^level-1‌

#### 局部坐标系

由于地球半径太大，直接画会有一米左右的误差，导致地形闪烁、骨骼动画物理模拟失真等问题，乘完view矩阵就会变小，所以在Cpu乘完再传给shader

每层地形块x的坐标范围为0-2^\(level+1\)-1,y的坐标范围为0-2^level-1，三层是128个块

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

### 受限地形四叉树分裂算法

1、相邻地形块层级相差太大会有T型裂缝问题，解决：

* 挡板（出现悬崖形状，纹理拉伸问题）
* 在粗糙的地形块边缘补上很多点（出现很多狭长的三角形）
* 受限分裂：大块限制小块分裂（突然转向的时候，粗糙块会限制原本精细的块分裂）
* 受限分裂：小块带动大块分裂

2、受限分裂算法

* 基于可见性把所有最高层的节点放入待分裂节点队列
* 设置父节点的邻居关系设置孩子节点的邻居关系
  * 如果相邻层数差不超过1，直接设成邻居
  * 不满足则对相邻块强制分裂
* 从待分裂队列中取出节点（宽度优先），进行分裂（深度优先分裂）

  * 所有节点初始为叶子节点（叶子结点为最终绘制的节点），分裂后被修改成内部节点
  * 如果判断该块不够精细，需要细分，首先求其四个孩子的包围盒（或从db里读取）判断可见性，设置孩子的邻居节点并放入待分裂节点队列
  * 
  每个地形块是独立的mesh吗？ 不是每次都从第三层开始分裂的，共享盘交接文档

### 地形数据管理

#### LRU

核心数据结构是两个双端队列一个hashmap，第一个双端队列相当于一个内存池\(dequeUnused\)，初始化时分配了所有节点，第二个队列保存了cache的时序信息\(dequeUsed\)，hashmap保证可以在O\(1\)的时间内，根据key取到地形块。quadTreeLRU保存了每个节点的子节点信息，从LRU中删除cache时会同时移除该节点的所有子节点（如果存在的话）

#### 异步地形数据请求

* MTDataCacheManager：

管理Cache请求的类。地形数据异步的从db里获取，纹理和数据用pbo传输数据到GPU，不包含异步调用，在主循环每次循环中串行调用（FrameExecute）

**线程池**

封装一个job类和一个jobQueue，和一个jobWorker,jobWorker包含一个jobQueue，对应线程池（boost thread group），初始化的时候将处理函数传递给它，jobWorker调用处理函数对job进行处理。

线程池初始化的时候就创建好若干线程，线程中反复检查工作队列数据，只要有数据就处理，直到处理工作完成。限制了每帧请求数目和每帧最大请求数目避免任务堆积。

**地形数据获取**

有三种dataCache:dem、tex、mask，当分裂地形四叉树获取某块的时候，如果在lru中找不到就会创建一个任务，每个cache有数据状态的属性（available, required, not available, data interpolated）。获取时把cache加入lru并设置其状态为required,

#### 地形数据库

mask db和boundingbox db生成。boundingbox db把一个块和直接子块打包在一起，查找起来快一些。由于地形块边界经纬度能算，所以boundingbox数据只有高度的minmax两个数据

### 地形编辑

#### 编辑数据同步

* 保存金字塔结构的数据：设置一个临时db，同步更新修改层的上层数据，异步更新其下层数据，保证实时交互效率；在保存状态下，真正将所有层数据存入与原引擎匹配的数据库。
* LRU数据管理：已编辑数据不能从LRU中换出，对相应Cache加锁
* 增加版本信息避免异步更新下层数据，请求父节点数据时获取到的不是最新的数据
* 注意边界问题以防止修改后出现T-junction

#### 改进

* 部分受限分裂的四叉树：远处看不清楚不一定要分裂
* 对飞行方向进行预测
* 线程数，每帧最大请求数调优

### 地形绘制

所有叶子节点作为绘制节点保存在一个节点列表里，一个节点一个节点的绘制，相邻块保存邻居信息，对于边界进行缝合。

## GamePlay

### Ecs架构

设计要点：

* 组件通信的方法
  * 通过消息系统（PX4）
  * 组件间相互引用
* 对象获取组件的方法
  * 对象创建组件（麻烦，不灵活）
  * ~~配置文件~~

优点：

1. C是纯数据，很方便做快照和回滚（面向数据的开发）
2. S只包含行为，容易进行并行优化处理
3. 方便处理同一system内对象间的交互关系
4. 数据局部化是获得高性能的一个主要方法，减少io瓶颈，最大化的利用cache的特性

#### object

* name和id
* rtti支持
  * static const Rtti TYPE
  * 保存本类名和基类的rtti对象指针
  * typedef baseType uper
* 反射
  * 类存储函数名到函数构造函数的映射关系，一个模板类实例保存同一个继承树（Actor、ActorComponent\)内所有类的构造函数

#### component

* sceneComponent
  * relative/world/local分别以parent、world和自己为坐标系原点
    * world是根据relative进行更新的，AddworldPos等相对当前worldpos设置位移时，首先用相对parent的缩放旋转对delta进行变换，从而计算正确的worldPos
    * 根据worldpos更新relativePos（InternalSetWorldPosition）需要首先得到正确的worldTranslation，然后进行relative变换的逆变换，将其变换到relative坐标系下
    * 设置local时要把delta变换到relative的坐标系中
    * 变换时子节点要递归的变换
  * add/set
  * 获取up/right/forword轴：用worldtransformation分别对x（-1，0，0，0）yz轴进行旋转
* GeometryComponent

#### 寻路



## 飞控项目

将Airsim中的无人机传感器仿真部分及无人机飞行控制库PX4集成到实验室自研的Viwo引擎中，

### AirSim是做什么的？

模拟器是无人机在真实世界里的视觉呈现，Airsim是飞控到渲染引擎的一个桥梁，支持了simpleFlight、px4、ardupilot等多种飞控库，接受用户输入的指令，经包装（mavlink）发送给飞控库，并接收解算结果，指导渲染引擎的渲染。Airsim用unreal自带的一些组件如WheeledVehicle、rotationMovementComponent等，提供了默认的无人机和无人车模型，使用户可以快速的即插即用的获得无人机无人车仿真体验。



px4可以直接运行在真机上，因此没有传感器信息和环境信息。

### 为了应用Airsim做了哪些工作？

飞行控制器的主要工作是将期望状态作为输入，利用传感器数据估计实际状态，然后驱动电机使实际状态接近期望状态。例如，对于四旋翼，可以指定所需的状态为横摇、俯仰和偏航。然后利用陀螺仪和加速度计估计实际的横摇、俯仰和偏航。然后产生适当的电机信号，使实际状态变为期望状态。模拟器利用飞行控制器产生的电机信号来计算每个执行器\(即四旋翼情况下的螺旋桨\)产生的力和推力。然后由物理引擎来计算飞行器的动力学特性。这进而生成模拟的传感器数据，并将其反馈给飞行控制器。

（[https://microsoft.github.io/AirSim/docs/flight\_controller/](https://microsoft.github.io/AirSim/docs/flight_controller/)）

* 为多人模式进行了适配
  * 原先AirSim没有用playerController，直接在Pawn的初始化里绑定了轴和key来接受输入。然后在把输入控制放到playerController里的过程中遇到了一些初始化顺序问题，通过重载postLogin等掌握关键时间节点的函数保证了初始化顺序正确
  * 多个ROS-Airsim端，通过多个端口连接到Unreal。在场景中放置playerStart后，在多玩家选项中设置玩家数量，unreal会在playerStart处根据gameMode中指定的默认playerController、pawn和HUD类创建player。controller和pawn都是首先在服务器上创建n个，如controller0，controller1，controller2，然后依次拷贝到每个客户端，每个客户端拥有的是controller0，controller0，controller0。重载GameMode的PostLogin函数获取Controller和Pawn在服务器创建并possess完成的时间节点。controller中保存simMode的指针，并在接受输入后通过simMode对vehicle进行控制，在PostLogin调用后，controller可以拿到pawn，然后委托SimHUD在客户端创建SimMode。
* Timer
  * 一个负责收发无人车无人机姿态信息和控制信息的类
  * 为无人车运动的速度设置一个小小的范围，每隔特定interval设置一次油门和刹车，检查无人车速度如果不在范围里就改变油门和刹车，使无人车速度稳定维持在期望值范围内。（非线性系统线性化？）

### 飞控为什么复杂？

飞行控制器的主要工作是将期望状态作为输入，利用传感器数据估计实际状态，然后驱动电机使实际状态接近期望状态。即，预测未来，修正当下。目前工业界经常用扩展卡尔曼滤波器（ekf）进行无人机多传感器融合。

* 涉及多个实体，运作机制复杂（模拟器、飞控模拟软件、地面站），多项配置（遥控器通道、遥控器校准、飞行模型选择、飞行模式选择，UDP网络配置）好比接口不同的两台机器，接口间还有相互依赖关系，需要参照px4和airsim的连线将线插到viwo上，需要对viwo、px4、airsim、mavlink等系统都有理解。
* 无法断点调试，模拟效果跟更新频率有关，涉及网络和多线程。
* 资料少，从Airsim切入，但Airsim文档有些过期
* 为了可扩展性封装实在太多层了。

### Unreal

#### server—client网络模型  

![](../.gitbook/assets/image%20%2869%29.png)

![](../.gitbook/assets/image%20%2894%29.png)

client去获取它没有权限获得的GameMode的时候只会得到空指针

* Gamemode：游戏规则，只存在在server
* GameState：在server和Client中间传递信息，从server复制到每个客户端的
* playerState:代表player的所有信息，也是拷贝给每个人的，这样每个client都知道所有玩家的信息。当切换地图或者断线重连的时候，playerState保存了之前的游戏状态

## 2D地图模块

飞机视景camera的direction投影在地球球面上的向量就是飞机在墨卡托投影地图上的方向。 将cameraDir投影至地球球面相当于将该向量投影在cameraEyeVector、经度方向、纬度方向正交构成的局部坐标系中。

这个局部坐标系由北极方向和eyeVector叉乘得到纬度方向，纬度方向和eyeVector叉乘再得到经度方向

### camera2D怎么工作?

工作流：

1. 3D相机更新位置
2. 计算rate：小地图宽度 / 相机所照到的实际宽度

   double rate = _\(PI \*_ g_coord.getShortAxis\(\) \* camera_-&gt;GetWindowHeight\(\)\) / \(40075453 \* _camera\_-&gt;GetElevation\(\) \*_ tan\(camera\_-&gt;GetFovy\(\) / 2 \* PI / 180\)\);

   1. 其中fovy是相机视角，默认为45度，角度\*pi/180是转弧度，40076km是赤道周长，shortAxis是地球半径，记为r，赤道周长记为c，公式可以转化为【\(pi\*r/c\)\*小地图高度 】/【相机高度\*tan（一半相机视角）】。
   2. 前半边中2\*pi\*r是圆周长，pi\*r是墨卡托下从南极到北极的距离，pi\*r/c是墨卡托坐标系下地图的长宽比，乘上小窗口height得到小窗口的理论宽度。后半边求得的是相机照到的实际宽度的一半。
   3. 数据源将地图分割成256\*256的小块

![](../.gitbook/assets/image%20%282%29.png)

![](../.gitbook/assets/image%20%2826%29.png)

![](../.gitbook/assets/image%20%2842%29.png)

## **Unity游戏开发**

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

## 泛问题

### 你用过最厉害的技术是什么？

gltf（巧妙）、ecs架构（反射、RTTI，逻辑庞大），px4飞控（复杂），地形分裂算法

### 你熟悉和用过的设计模式？

单例（懒汉饿汉）、工厂\(游戏开发）、桥接、命令模式（gameplay\)、观察者模式（viwo）、原型模式（Unreal\)、mvc、ecs

### 你想问的问题？

~~入职前准备，从哪得知面试评价，~~入职后承担的工作和项目，实际工作面临的困难，怎么解决

部门的战略和未来发展

团队、公司现在面临的最大挑战是什么？

离职情况和晋升路径

绩效考核，如何提升员工幸福感，如何帮新员工快速上岗。

缺点 

自己的成就动机比较高，让你总有冲动去调整更高的挑战与目标。当领导交给你一份工作，无论因为组织内部如流程、授权的原因，还是个人的能力原因，没有完成的，\(开始说缺点了:\)你就会很沮丧，很不开心，感觉到很受挫。你就很羡慕那些在工作中无论遇到什么挫败感，总能保持心如止水的人，我希望自己努力掌握心理自我调适的能力 

优点 

没有拖延症，比较有责任感，沟通能力比较强，有自我调节能力，性格开朗。比较有条理，爱写文档和总结

### 职业规划

兴趣、价值观和动机和职业匹配，愿意为公司奋斗。家庭和个人生活稳定，可以为工作提供良好的保障。通过自学尽快成长，承担工作职责。



1、组织和策划协会日常活动开展。2、召开例行周会，维护协会各职能部门运行。3、组织暑期远途骑行活动。4、协会评奖评优工作。



