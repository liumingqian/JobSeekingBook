# 内存管理

## 对象构造

1. 父类对声明时进行初始化的成员变量进行赋值
2. 子类对声明时进行初始化的成员变量进行赋值
3. 父类初始化列表
4. 父类构造函数
5. 子类初始化列表
6. 子类构造函数

### New/Delete

#### new/delete操作符

new完成两个功能：

* 用operator new分配内存
* 在分配的内存行构造函数初始化

#### operator new

返回值类型是void\*，返回一段未初始化的内存。

```cpp
void * operator new(size_t size);
```

完成标准的内存分配。要实现不同的内存分配行为要重载operator new，new operator和placement new都是不能重载的

#### placement new

 是operator new的一个重载版本,在用户指定的内存位置上构建新的对象，这个构建过程不分配内存，只需要调用对象的构造函数，并返回分配好的内存的指针好处 在已分配好的内存上进行对象的构建，构建速度快。 可以提升buffer的效率，省去调用默认构造函数_（vector的emplace\_back方法）_。 已分配好的内存可以反复利用，有效的避免内存碎片问题。

```cpp
void buffer = malloc(sizeof(ClassA)); 
ClassA ptr = new(buffer)ClassA();
```

#### std::allocator

std::allocator 类模板是所有标准库容器所用的默认分配器。有allocate（分配未初始化的内存）、deallocate、construct（在分配的内存中构造对象）、destroy（析构在已分配的内存中构造的对象）等方法。

#### malloc/free和new/delete的区别 

malloc/free是C/C++语言的标准库函数，new/delete是C++的运算符。new和delete可以自动调用构造和析构函数。

### sizeof

* size\(\)是string的一个函数，不是一个概念
* 可以求静态分配的数组的长度，不能求动态分配的内存大小
* 不能用于void

#### 内存对齐

1、结构体的大小等于结构体内最大成员大小的整数倍 

2、 结构体内的成员的首地址相对于结构体首地址的偏移量是其类型大小的整数倍，比如说double型成员相对于结构体的首地址的地址偏移量应该是8的倍数。

{% tabs %}
{% tab title="类对象" %}
c++定义一个空的类CTest，CTest没有定义任何成员变量和成员函数，在32位机器上：

* 给CTest添加构造函数，再对CTest求sizeof，结果为1.（不能为0\)
* 给CTest添加虚函数，再对CTest求sizeof，结果为4.（一个指向虚函数表的指针\)
* 有成员变量的话注意内存对齐
{% endtab %}

{% tab title="内存对齐" %}
```cpp
#pragma pack(4)
struct B
{
  char b;
  int a;
  short c;
};
#pragma pack()
```

内存对齐为12字节
{% endtab %}

{% tab title="sizeof 指针和数组" %}
```cpp
char * s1 = "hellohwc";//指针，32位环境为4字节，64位环境为8字节
char s2[] = "hellohwc";//数组，8+1=9
char s3[100];//数组，100
```
{% endtab %}
{% endtabs %}

### 用指针操作地址

{% tabs %}
{% tab title="例1" %}
{% code-tabs %}
{% code-tabs-item title="用指针对地址进行操作" %}
```cpp
struct A

{
	bool b;
	int arr[2];
	int i;
	int j;
};

	A a;
	a.b = false;
	a.arr[0] = 1;
	a.arr[1] = 2;
	a.i = 20;
	a.j = 30;
	*(a.arr + 1) = 40;
	A*p = 0;
	unsigned int q = (unsigned int)(&p->i);
	(*(int*)((char*)&a + q)) = -50;	
```
{% endcode-tabs-item %}
{% endcode-tabs %}

程序执行完后：

![](../../.gitbook/assets/image%20%2824%29.png)

p的地址为0x00000000，&p-&gt;i取了i的地址，但没有用这块内存，所以不会出错，i的地址为0x00000012。将a转换为char指针后，再加12字节，得到的是a.i所在的内存，并对这块内存解析为int进行了赋值。
{% endtab %}

{% tab title="例2" %}
{% code-tabs %}
{% code-tabs-item title="指针含义辨析" %}
```cpp
const char* str[3] = {"abc","def", "ghi"};
const char* ps = str[0];//ps是一个指向const char数组的指针，ps本身不是const
cout << ++ps << endl;//输出“bc”
```
{% endcode-tabs-item %}
{% endcode-tabs %}
{% endtab %}
{% endtabs %}

