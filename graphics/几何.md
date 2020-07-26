# 几何

### 点乘

a•b = \|a\|\|b\|cosθ = a1b1+a2b2+……anbn

点乘的结果是一个标量，等于向量大小与夹角的cos值的乘积，其结果的几何意义是一个向量在另一个向量上投影的长度，如果a和b都是单位向量，那么点乘的结果就是其夹角的cos值。

![&#x5411;&#x91CF;&#x6295;&#x5F71;&#xFF0C;&#x6EE1;&#x8DB3;&#x4EA4;&#x6362;&#x5F8B;](../.gitbook/assets/image%20%2832%29.png)

![&#x6EE1;&#x8DB3;&#x5206;&#x914D;&#x7387;](../.gitbook/assets/image%20%286%29.png)

### 叉乘

向量a 和b 叉乘, 得到一个垂直于a 和b的向量a ×b , 它的方向由右手螺旋法则确定, 它的长度几何意义是a和b张开的平行四边形的面积。

![&#x5176;&#x5B9E;&#x5E94;&#x5F53;&#x4E3A;\|a\|&#xB7;\|b\|&#xB7;sin&#x3B8;&#xB7;n&#xFF0C;\|a\|&#xB7;\|b\|&#xB7;sin&#x3B8;&#x8868;&#x5927;&#x5C0F;&#xFF0C;&#x5411;&#x91CF;n&#x8868;&#x6CD5;&#x7EBF;&#x65B9;&#x5411;](../.gitbook/assets/image%20%2841%29.png)

![](../.gitbook/assets/image%20%2825%29.png)

注意：U×V和V×U,大小相等，方向因为右手法则旋转方向相反而相反。

![](../.gitbook/assets/image%20%2812%29.png)

### 四元数

* 不直观
* 节省空间，插值方便，不会死锁

![](../.gitbook/assets/image%20%2861%29.png)

![](../.gitbook/assets/image%20%2878%29.png)

其中α为旋转的角度，cos

![](../.gitbook/assets/image%20%2835%29.png)

### 点到直线的距离

* 点到直线距离

![](../.gitbook/assets/image%20%2867%29.png)

* 用叉乘求点到直线距离

设有一点p，和直线上两点构成向量pa，pb，则向量ab为pb-pa。pa×ab为这个平行四边形面积，等于\|pa\|\*\|ab\|\*sin\(pab\)，而\|ab\|\*sin\(pab\)即为这个平行四边形的高

### 判断点在三角形内

![](../.gitbook/assets/image%20%2850%29.png)

* 叉乘

判断PA×PB，PB×PC，PC×PA是否正负相同，正负相同表明对于p点来说这三个点的顺逆时针顺序相同。（向量u×v和v×u方向相反）

* 点乘

用点乘判断PA、PB、PC的夹角是否和为360

### 判断直线与圆是否相切相交

[判断点到直线距离](ji-he.md#dian-dao-zhi-xian-de-ju-li)是否等于圆的半径

### 判断点是否在多边形内部

![](../.gitbook/assets/image%20%2860%29.png)

![](../.gitbook/assets/image%20%2814%29.png)

### 范围搜索和最邻近搜索

k-d树（k-dimentional tree）,主要应用于多维空间关键数据的搜索。kd树的构建是从当前维度的关键值中找出中值作为分隔平面，并对平面的左右自空间递归的进行划分。

#### 在二维地图上寻找视野范围内的角色

不均匀分裂的四叉树（三维中是八叉树）

kd树：[ref](https://blog.csdn.net/google19890102/article/details/54291615)

对于二维平面来说只有两个关键值，可以轮流使用x和y作为每一轮划分平面的依据。搜索时从叶子节点向上回溯，不断将可能的节点添加进搜索队列，直到搜索队列为空。

R树（三维中是BVH树）

![](../.gitbook/assets/image%20%2870%29.png)

![](../.gitbook/assets/image%20%2854%29.png)

