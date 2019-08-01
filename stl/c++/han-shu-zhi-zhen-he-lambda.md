# 函数指针和Lambda



```cpp
int add(int nLeft,int nRight);//函数定义  

//函数指针
//声明函数指针只需用指针替换函数名即可
int (*pf)(int,int);//未初始化  
//注意：*pf两端的括号必不可少，否则若为如下定义：
int *pf(int,int);//此时pf是一个返回值为int*的函数，而非函数指针 

pf = add;//赋值
pf(100,100);//与普通函数用法相同
```



### lambda表达式

```cpp
//capture是捕获列表；params是参数列表；opt是函数选项；ret是返回值类型；body是函数体
　[captrue] (params) opt -> ret {body};
 //如
auto f = [](int a) -> int {return a + 1; };

//捕获
[]不捕获任何变量；
[&]捕获外部作用域所有变量，并作为引用在函数体使用（按引用捕获）；
[=]捕获外部作用域作用变量，并作为副本在函数体使用（按值捕获）；
[=,&a]按值捕获外部作用域所有变量，并按引用捕获a变量；
[b]按值捕获b变量，同时不捕获其他变量；
[this]捕获当前类中的this指针，让lambda拥有和当前类成员函数同样的访问权限，
如果已经使用了&或者=，就默认添加此选项。
捕获this的目的是可以在lambda中使用当前类的成员函数和成员变量。
```

### std::function

### std::bind

