# 概率论

### 洗牌算法

1. 初始化原始数组和新数组，原始数组长度为n\(已知\)；
2. 从还没处理的数组（假如还剩k个）中，随机产生一个\[0, k\)之间的数字p（假设数组从0开始）；
3. 从剩下的k个数中把第p个数取出；
4. 重复步骤2和3直到数字全部取完；
5. 从步骤3取出的数字序列便是一个打乱了的数列。

   下面证明其随机性，即每个元素被放置在新数组中的第i个位置是1/n（假设数组大小是n）。

   证明：一个元素m被放入第i个位置的概率P = 前i-1个位置选择元素时没有选中m的概率 \* 第i个位置选中m的概率，即

![](../.gitbook/assets/image%20%2865%29.png)

作者：lyz\_cs 来源：CSDN 原文：

[ref:https://blog.csdn.net/qq\_26399665/article/details/79831490](https://blog.csdn.net/qq_26399665/article/details/79831490) 

[https://mp.weixin.qq.com/s?\_\_biz=Mzg2NzA4MTkxNQ==&mid=2247485699&idx=1&sn=05d48f11e437b6983d6d6650e1f65710&chksm=ce4042d7f937cbc1ae9399fa954a4aec300be4f00183520576bcd7d25a9e9567bba455089c4f&scene=0&xtrack=1&key=6049ea3782f5de5456ff6e580138323be3173c7cf422de16373a5b43d527ea8f7f4949d75017b683e0c8a5c568295877acfec232abf2ed0bde0cd487e7f68c21b590e9a7de07b26576df49d865aca396&ascene=1&uin=MTIyODM5ODk2NA%3D%3D&devicetype=Windows+10&version=62060833&lang=zh\_CN&pass\_ticket=cMBEk4XC9FbEO9it9f4Bq3fcavODQ6QPNzr4Bv7vPTDIcKRUBANNWCvzSsAcp%2FSG](https://mp.weixin.qq.com/s?__biz=Mzg2NzA4MTkxNQ==&mid=2247485699&idx=1&sn=05d48f11e437b6983d6d6650e1f65710&chksm=ce4042d7f937cbc1ae9399fa954a4aec300be4f00183520576bcd7d25a9e9567bba455089c4f&scene=0&xtrack=1&key=6049ea3782f5de5456ff6e580138323be3173c7cf422de16373a5b43d527ea8f7f4949d75017b683e0c8a5c568295877acfec232abf2ed0bde0cd487e7f68c21b590e9a7de07b26576df49d865aca396&ascene=1&uin=MTIyODM5ODk2NA%3D%3D&devicetype=Windows+10&version=62060833&lang=zh_CN&pass_ticket=cMBEk4XC9FbEO9it9f4Bq3fcavODQ6QPNzr4Bv7vPTDIcKRUBANNWCvzSsAcp%2FSG)

### 二项分布

重复n次伯努利实验，每次实验独立，每次实验只有两种结果，两种结果互斥，且结果事件的概率在整个实验中不变

期望E为np，方差D为np（1-p）

