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

### std::function

### std::bind

