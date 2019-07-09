# 智能指针

### Unique\_ptr

```cpp
std::unique_ptr<Resource> res1(new Resource); // Resource created here
std::unique_ptr<Fraction> f1 = std::make_unique<Fraction>(3, 5);//动态创建有参数的类
auto f2 = std::make_unique<Fraction[]>(4);//动态创建一个长度为4的数组

//可安全的从函数中作为值返回
std::unique_ptr<Resource> createResource()
{
     return std::make_unique<Resource>();
}
 
 //像函数中传值时可以这样用，此时资源的所有权被转移到takeOwnership()，
 //因此资源在takeOwnership()的末尾而不是main()的末尾被销毁。
 //但是大多数情况下，您不希望函数拥有资源的所有权。虽然您可以通过引用传递std::unique_ptr
 //(这将允许函数使用对象而不假设拥有所有权)，但是您应该只在调用的函数可能更改或更改正在管理的对象时才这样做
takeOwnership(std::move(ptr)); 
//更好的做法
useResource(ptr.get());//直接传递资源
   
```

### 自己实现

智能指针是模板类，需要实现的内容包括：

* raw 指针
* 一个int\*类型的计数器
* 拷贝构造函数和赋值构造函数
* 重载解引用符号\*
* 重载取成员符号-&gt;

