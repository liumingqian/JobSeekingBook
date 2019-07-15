# STL标准模板库

## STL概览

![](../../.gitbook/assets/image%20%2842%29.png)

![](../../.gitbook/assets/image%20%2883%29.png)

![](../../.gitbook/assets/image%20%2882%29.png)

![](../../.gitbook/assets/image%20%2873%29.png)

## 迭代器

（1）迭代器不是指针，是类模板，表现的像指针。只是模拟了指针的一些功能，通过重载了指针的一些操作符，-&gt;,\*,++ --等封装了指针，是一个“可遍历STL（ Standard Template Library）容器内全部或部分元素”的对象， 本质是封装了原生指针，是指针概念的一种提升（lift），提供了比指针更高级的行为，相当于一种智能指针，他可以根据不同类型的数据结构来实现不同的++，--等操作；

（2）迭代器返回的是对象引用而不是对象的值

#### 迭代器失效

对于关联容器来说插入不会导致迭代器失效，但如在遍历过程中删除，需要以erase\(iter++\)的方式删除迭代器。

如vector、deque等数组实现的容器，erase操作会导致其后所有iterator失效。注意vector的push\_back操作也可能导致内存变动，迭代器可能会失效。

ref:h[ttps://blog.csdn.net/gogokongyin/article/details/51206225](https://blog.csdn.net/gogokongyin/article/details/51206225)

[https://blog.csdn.net/qq\_37964547/article/details/81160505](https://blog.csdn.net/qq_37964547/article/details/81160505)

