# Vector

#### 使用

{% code-tabs %}
{% code-tabs-item title="vector的初始化" %}
```cpp
vector<int> list2(list）;
vector<int> list2=list;

//list2初始化为list的拷贝
vec//初始化为列表中元素的拷贝
vector<int> list={1,2,3,4};

vector<int> list(list2.begin(),list.end()-2);

//list包含7个元素，用缺省值初始化
vector<int> list(7);

//list包含7个元素，用3初始化
vector<int> list(7,3);

sort(list.begin(),list.end());
unique(list.begin(),list.end());
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% code-tabs %}
{% code-tabs-item title="迭代器使用" %}
```cpp
for (vector<void *>::iterator it = v.begin(); it != v.end(); it ++) 

    if (NULL != *it) 
    {
        delete *it; 
        *it = NULL;
    }
v.clear();
```
{% endcode-tabs-item %}
{% endcode-tabs %}

#### vector内存管理

ref:[C++ STL中的vector的内存分配与释放](https://www.cnblogs.com/biyeymyhjob/archive/2012/09/12/2674004.html)

**释放**：由于vector的内存占用空间只增不减，比如首先分配了10,000个字节，然后erase掉后面9,999个，留下一个有效元素，但是内存占用仍为10,000个。所有内存空间是在vector析构时候才能被系统回收。empty\(\)用来检测容器是否为空的，clear\(\)可以清空所有元素。但是即使clear\(\)，vector所占用的内存空间依然如故，无法保证内存的回收。如果需要空间动态缩小，可以考虑使用deque。如果vector，可以用swap\(\)来帮助你释放内存。swap\(\)是交换函数，使vector离开其自身的作用域，从而强制释放vector所占的内存空间。



**构造**：emplace\_back能在vector当前末尾就地通过参数构造对象，不需要拷贝或者移动内存，相比push\_back能更好地避免内存的拷贝与移动



{% hint style="danger" %}
Vector中保存原始指针的时候，Vector 析构的时候不会释放指针指向的堆对象，但是可以释放智能指针所指对象
{% endhint %}

