# 智能指针

### Unique\_ptr

直接向函数中传递是不行的 use\(ptr\);//不work。直接向函数中传递resource更好。Unique\_ptr可以管理数组

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

//！以下是会出现问题的写法！：
//不要多次释放同一个资源
Auto_ptr1<Resource> res1(new Resource);
Auto_ptr1<Resource> res2(res1); 

//不要用同一个资源构造多个智能指针
Resource *res = new Resource;
std::unique_ptr<Resource> res1(res);
std::unique_ptr<Resource> res2(res);

//构造智能指针后不要再操作raw pointer内存
Resource *res = new Resource;
std::unique_ptr<Resource> res1(res);
delete res;
```

#### make\_unique

make\_unique只是把参数完美转发给要创建对象的构造函数，更推荐make\_unique而不是自己new，看着更清楚，并解决了一个异常安全问题

### shared\_ptr

```cpp
Resource *res = new Resource;
std::shared_ptr<Resource> ptr1(res);
std::shared_ptr<Resource> ptr1(new Resource());
//注意！！应当从shared_ptr构造shared_ptr!,而不能重复从普通指针构造ptr
//如果std::shared_ptr<Resource> ptr2(res);这两个指针的计数是相互独立的！
std::shared_ptr<Resource> ptr2(ptr1);

//用make_shared创建
auto ptr1 = std::make_shared<Resource>();
//拷贝创建
auto ptr2 = ptr1;
//从unique_ptr move过来
```

### weak\_ptr

weak\_ptr只能从shared\_ptr或者weak\_ptr构造，它是弱引用，不能直接管理资源，也不参与资源引用计数，解决了智能指针循环引用无法释放的问题。

![](../../.gitbook/assets/image%20%2884%29.png)

 因为weak\_ptr本身不会增加引用计数，所以它指向的对象可能在用的时候已经被释放了，所以在用之前需要使用`expired`函数来检测是否过期，然后使用`lock`函数来获取其对应的`shared_ptr`对象，然后进行后续操作。

### 实现

智能指针是模板类，需要实现的内容包括：

* raw 指针
* 一个int\*类型的计数器
* 拷贝构造函数和赋值构造函数
* 重载解引用符号\*
* 重载取成员符号-&gt;

unique\_ptr:自定义删除函数

