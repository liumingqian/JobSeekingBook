# 关键字

### static

constexpr

### mutable

用mutable标记的成员变量永远是可变的，可以突破const的限制，在const函数里也是可以修改的。

### extern

* extern声明的函数和变量可以在别的cpp中调用

```cpp
//.h文件中
extern int a;
//.cpp文件中
int a = 10;
```

* 告诉编译器这部分代码按C编译，为了C++正确调用C代码

### explicit

防止隐式转换，只能显示的调用函数，可以预防错误的传参，使接口更加清晰（effective C++18）

