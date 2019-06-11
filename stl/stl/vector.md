# 顺序容器

## String

### 使用

{% code-tabs %}
{% code-tabs-item title="从字符串和字符数组构造" %}
```cpp
int main()
{
	const char* str1 = "attr";
	string str2("attr");
	string str3(str1);
	const char* str4 = 
"attr";
 ////str1和str4一样，其他两个都不一样，常量字符串“attr”保存在全局常量区
	cout << "str1的地址:" << (void*)str1 << endl;
	cout << "str2的地址:" << (void*)str2.c_str() << endl;
	cout << "str3的地址:" << (void*)str3.c_str() << endl;
	cout << "str4的地址:" << (void*)str4 << endl;
	return 0;
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% code-tabs %}
{% code-tabs-item title="常用功能" %}
```cpp
//截取子串
string substr(size_t pos = 0, size_t len = npos);

//合并
//注意参数和返回值都是引用！
string& append(const string& str);

//IO
//参数：std::cin,str,终结符，遇到该字符停止操作
istream& getline(istream &is , string &str , char delim );
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% code-tabs %}
{% code-tabs-item title="algorithm" %}
```cpp
#include <algorithm> 
//查找
//find_first_of 函数和find函数最大的区别就是如果在一个字符串str1中查找另一个字符串str2，
//如果str1中含有str2中的任何字符，则就会查找成功，而find则不同；
//同族函数还有rfind、find_first_of、find_last_of
//find_first_not_of、find_last_not_of
//**注意**：返回值为size_t(unsigned int)
size_t find(const string& str, size_t pos = 0);
size_t find(const char* s, size_t pos = 0);
size_t find(const char* s, size_t pos, size_type n);//之比较前n个字符

//反转字符串
std::reverse(str.begin(),str.end());
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% code-tabs %}
{% code-tabs-item title="大小写转换" %}
```cpp
#include <ctype.h>：

int tolower ( int c );
int toupper ( int c );
```
{% endcode-tabs-item %}
{% endcode-tabs %}

### 与其他类型的转换 

1、格式化字符串

```cpp
 //将data转换为字符串
 sprintf(str,"%d",data);
 //连接字符串s1和s2
 sprintf(str,"%s %s",s1,s2);
 
  char s[15] = "123.432,432";
  double f1;
  int f2;
  sscanf(s, "%lf,%d", &f1, &f2);//123.432 432
```

### stringStream

```cpp
#include <sstream>

stringstream ss;//默认构造函数，从当前流中内容构建stringstream
stringstream ss(str);//从字符串str构造stringstream

ss.str(s);//用字符串s填充ss

ss << N;//向ss流中输入值
ss >> str;//向str中写入

ss.clear();//清除流中内容
```

ref: [https://blog.csdn.net/u013834525/article/details/82533935](https://blog.csdn.net/u013834525/article/details/82533935)

## Vector

vector的内部数据结构是数组，vector之间的赋值是深拷贝。

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

//首尾,返回元素引用
v.front();
v.back();
//返回第一个元素迭代器
v.begin();
//返回最后一个元素后面的位置
v.end();
//返回指向最后一个元素的逆序迭代器
v.rbegin();
//返回指向第一个元素前面的逆序迭代器
v.rend();
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% code-tabs %}
{% code-tabs-item title="algorithm" %}
```cpp
#include <algorithm>  

//反转
std::reverse(myvector.begin(),myvector.end());   
//排序
std::sort(v.begin(),v.end());
//删除
//删除返回指向下一个元素的迭代器
iterator erase(iterator position);
iterator erase(iterator first, iterator last);
//删除重复元素
//unique不实际上删除元素，而是把所有无重复的元素排到数组前面，
//unique返回的迭代器指向超出无重复的元素范围末端的下一个位置
c.erase(unique(c.begin(), c.end())c.end());

```
{% endcode-tabs-item %}
{% endcode-tabs %}

#### vector内存管理

```cpp
//分配内存并初始化
//用resize分配内存效率远高于push_back，但resize不会对vector中已经存在的元素进行重新初始化
v.resize(5);
v.resize(5,1);

//只是预留了空间，设置了capacity，并没实际分配内存
v.reserve();
```

ref:[C++ STL中的vector的内存分配与释放](https://www.cnblogs.com/biyeymyhjob/archive/2012/09/12/2674004.html)

**释放**：由于vector的内存占用空间只增不减，比如首先分配了10,000个字节，然后erase掉后面9,999个，但是内存占用仍为10,000个。所有内存空间在vector析构时才能被系统回收。empty\(\)用来检测容器是否为空的，clear\(\)可以清空所有元素。但是即使clear\(\)，vector所占用的内存空间依然如故，无法保证内存的回收。如果需要空间动态缩小，可以考虑使用deque。如果vector，可以用swap\(\)来帮助你释放内存。swap\(\)是交换函数，使vector离开其自身的作用域，从而强制释放vector所占的内存空间。

**构造**：emplace\_back能在vector当前末尾就地通过参数构造对象，不需要拷贝或者移动内存，相比push\_back能更好地避免内存的拷贝与移动

{% hint style="danger" %}
Vector中保存原始指针的时候，Vector 析构的时候不会释放指针指向的堆对象，但是可以释放智能指针所指对象
{% endhint %}

## List

list是非连续存储的双向链表，不支持根据下标随机存取，但增删元素能在常数时间内完成。

除了类似vector的用法外，list有push\_front、pop\_front、front等方法

```cpp
lst1.splice(p1, lst2, p2, p3); //将[p2, p3)插入p1之前，并从 lst2 中删除[p2,p3)
```

list可以用于解决约瑟夫环问题。

## Deque

连续存储的双端可变长数组。在功能上合并了list和vector，但占用内存比较大。

## priority\_queue

priority\_queue无法遍历。

```cpp
//priority_queue模板有三个参数，默认存储元素的容器是vector，建立的是大根堆

template <typename T, typename Container=std::vector<T>, 
                      typename Compare=std::less<T>> class priority_queue
                      
//可以用任何有 front()、push_back()、pop_back()、size()、empty()方法的容器代替vector
std::priority_queue<std::string, std::deque<std::string>> words
//改变排序规则
std::priority_queue<std::string, std::vector<std::string>,std: :greater<std::string>> 
```

## Stack/Queue

stack和queue是两种限定操作的容器适配器，queue的容器需要支持以下的方法：

* empty
* size
* front
* back
* push\_back
* pop\_front

stack容器需要支持：

* empty
* size
* back
* push\_back
* pop\_back

stack 模板类需要两个模板参数，一个是元素类型，一个容器类型，但只有元素类型是必要 的，在不指定容器类型时，默认的容器类型为deque。

Queue的默认容器类型为deque，限制操作为先进先出

