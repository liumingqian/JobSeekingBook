# 内存管理

### sizeof

{% tabs %}
{% tab title="例1" %}
c++定义一个空的类CTest，CTest没有定义任何成员变量和成员函数，在32位机器上：

* 给CTest添加构造函数，再对CTest求sizeof，结果为1.（不能为0\)
* 给CTest添加虚函数，再对CTest求sizeof，结果为4.
{% endtab %}

{% tab title="Second Tab" %}

{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="例1" %}
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

程序执行完后：

![](../../.gitbook/assets/image%20%2820%29.png)

p的地址为0x00000000，&p-&gt;i取了i的地址，但没有用这块内存，所以不会出错，i的地址为0x00000012。将a转换为char指针后，再加12字节，得到的是a.i所在的内存，并对这块内存解析为int进行了赋值。
{% endtab %}

{% tab title="Second Tab" %}

{% endtab %}
{% endtabs %}

#### 内存对齐

{% tabs %}
{% tab title="例1" %}
```text
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

{% tab title="Second Tab" %}

{% endtab %}
{% endtabs %}

