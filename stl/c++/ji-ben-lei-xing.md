# 类型和运算符

### 基本类型

| 类型 | 字节数 | 位数 | 取值范围 |
| :--- | :--- | :--- | :--- |
| byte | 1 | 8 | -128 ~ 127 |
| bool | 1 | 1\(前7位是0\) | 0/1 |
| char | 1 | 8 |  |
| short | 2 | 16 | -32768 ~ 32767 |
| int | 4 | 32 | -2147483648 ~ 2147483647（-2^31 ~ 2^31-1） |
| unsigned int | 4 | 32 | 0 ~ 4,294,967,295（0~2^32-1） |
| long | 8 | 64 | -2^63 ~ 2^63-1 |
| float | 4 | 32 |  |
| double | 8 | 64 |  |

### 类型转换

* unsigned：无符号数和有符号数进行运算，无论谁在前谁在后，都会先转换为无符号数，对于负数会发生溢出，如-1对应的无符号数是4 294 967 295（32位无符号数最大值）。

{% hint style="danger" %}
注意：当用size\(\)进行计算时，size\(\)的返回值为size\_t类型，是无符号整数，需要避免计算结果为负数。
{% endhint %}

#### dynamic\_cast

除dynamic\_cast之外的三种转换的都是编译期进行的，只有dynamic\_cast是运行期进行的，因此可以保证向下转换（父类转指针子类指针）的安全性，转换失败返回null

**static\_cast**

编译期进行，不能保证下行转换的安全，即使转换失败也不返回null

### union和struct

{% code-tabs %}
{% code-tabs-item title="union的应用" %}
```cpp
//数据的多种解释方式
union
{
    T m_fVec[3];
    struct
    {
        T X, Y, Z;
    };
    struct
    {
        T x, y, z;
    };
};
//测试大小端（
```
{% endcode-tabs-item %}
{% endcode-tabs %}

### 运算符和表达式

#### 运算符优先级

单目运算符（!~++ --\)&gt;算术运算符（+-\*/%\)&gt;按位运算符（&^\|&&\|\|\)&gt;三目运算符（a?b:c\)&gt;赋值运算符

#### 运算符重载

```cpp
T& T::operator=(const T &myStr)//返回引用才可以连等赋值；传入常量引用，避免调用复制构造函数
{
    if（this==&str)      //判重
        return *this;
    delete []m_data;     //分配新内存之前释放旧内存避免泄露
    m_data=nullptr;
    
    m_data=new char[strlen(myStr.m_data)+1];
    strcpy(m_data,str.m_data);
    
    return *this;
}
```

### NAN（not a number）

*  0和正负无穷进行的加减乘除
* 对负数开偶次根
*  对负数进行[对数](https://baike.baidu.com/item/%E5%AF%B9%E6%95%B0)运算

