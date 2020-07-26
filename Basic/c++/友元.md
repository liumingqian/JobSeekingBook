# 友元

### 友元函数

```cpp
class Time
{
public:
         friend Time operator*(double m, const Time & t);//友元函数
}
```

友元函数在类的声明中，但不是类的成员函数，但与类的成员函数有相同的访问权限。

#### 重载运算符

```cpp
//双目运算符的重载，成员函数重载带一个参数，友元函数重载带两个参数。
//如下面的函数如果不加friend关键字，会报“此运算符函数的参数太多”错误，因为成员函数的运算符重载函数
//隐式的带一个this参数
friend std::ostream & operator<<(std::ostream & os, const Time & t)
{
    os<<"hours: "<<t.hours<<", minutes: "<<t.minutes<<std::endl;
    return os;
}
```

[运算符重载](ji-ben-lei-xing.md#yun-suan-fu-zhong-zai)

### 友元类

友元类的注意事项：

\(1\) 友元关系不能被继承。  
\(2\) 友元关系是单向的，不具有交换性。若类B是类A的友元，类A不一定是类B的友元，要看在类中是否有相应的声明。  
\(3\) 友元关系不具有传递性。若类B是类A的友元，类C是B的友元，类C不一定是类A的友元，同样要看类中是否有相应的申明。

