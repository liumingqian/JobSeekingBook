# 几何

### 点乘

a•b = \|a\|\|b\|cosθ = a1b1+a2b2+……anbn

点乘的结果是一个标量，等于向量大小与夹角的cos值的乘积，如果a和b都是单位向量，那么点乘的结果就是其夹角的cos值。

![&#x5411;&#x91CF;&#x6295;&#x5F71;&#xFF0C;&#x6EE1;&#x8DB3;&#x4EA4;&#x6362;&#x5F8B;](../../.gitbook/assets/image%20%2822%29.png)

![&#x6EE1;&#x8DB3;&#x5206;&#x914D;&#x7387;](../../.gitbook/assets/image%20%284%29.png)

### 叉乘

向量a 和b 叉乘, 得到一个垂直于a 和b的向量a ×b , 它的方向由右手螺旋法则确定, 它的长度是a和b张开的平行四边形的面积。

![&#x5176;&#x5B9E;&#x5E94;&#x5F53;&#x4E3A;\|a\|&#xB7;\|b\|&#xB7;sin&#x3B8;&#xB7;n&#xFF0C;\|a\|&#xB7;\|b\|&#xB7;sin&#x3B8;&#x8868;&#x5927;&#x5C0F;&#xFF0C;&#x5411;&#x91CF;n&#x8868;&#x6CD5;&#x7EBF;&#x65B9;&#x5411;](../../.gitbook/assets/image%20%2826%29.png)

![](../../.gitbook/assets/image%20%2817%29.png)

注意：U×V和V×U,大小相等，方向因为右手法则旋转方向相反而相反。

![](../../.gitbook/assets/image%20%288%29.png)

### 点到直线的距离

* 点到直线距离

![](../../.gitbook/assets/image%20%2843%29.png)

* 用叉乘求点到直线距离

设有一点p，和直线上两点构成向量pa，pb，则向量ab为pb-pa。pa×ab为这个平行四边形面积，等于\|pa\|\*\|ab\|\*sin\(pab\)，而\|ab\|\*sin\(pab\)即为这个平行四边形的高

### 判断点在三角形内

![](../../.gitbook/assets/image%20%2833%29.png)

* 叉乘

判断PA×PB，PB×PC，PC×PA是否正负相同，正负相同表明对于p点来说这三个点的顺逆时针顺序相同。（向量u×v和v×u方向相反）

* 点乘

用点乘判断PA、PB、PC的夹角是否和为360

### 判断直线与圆是否相切相交

[判断点到直线距离](ji-he.md#dian-dao-zhi-xian-de-ju-li)是否等于圆的半径

### 判断点是否在多边形内部

![](../../.gitbook/assets/image%20%2839%29.png)

![](../../.gitbook/assets/image%20%289%29.png)

