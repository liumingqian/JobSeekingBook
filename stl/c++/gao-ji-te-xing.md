# 高级特性

## C++11

### std::move

move语义意味着该类将转移对象的所有权，而不是复制对象。

std::move解决了以下问题： 

1、产生资源的函数应当返回资源，但智能指针会在离开作用域时析构资源

```cpp
??? generateResource()
{
     Resource *r = new Resource;
     return Auto_ptr1(r);
}

//move语义的实现在std::auto_ptr中通过重载赋值构造和拷贝构造函数来实现，如：
Auto_ptr2(Auto_ptr2& a) // note: not const
{
	m_ptr = a.m_ptr; // transfer our dumb pointer from the source to our local object
	a.m_ptr = nullptr; // make sure the source no longer owns the pointer
}

Auto_ptr2& operator=(Auto_ptr2& a) // note: not const
{
	if (&a == this)
		return *this;
 
	delete m_ptr; // make sure we deallocate any pointer the destination is already holding first
	m_ptr = a.m_ptr; // then transfer our dumb pointer from the source to the local object
	a.m_ptr = nullptr; // make sure the source no longer owns the pointer
	return *this;
}
```

2、解决拷贝带来开销的问题，延长右值生命期

```cpp
//①
template<class T>
void myswap(T& a, T& b) 
{ 
  T tmp { std::move(a) }; // invokes move constructor
  a = std::move(b); // invokes move assignment
  b = std::move(tmp); // invokes move assignment
}

//②
v.push_back(std::move(str));
```

## 

