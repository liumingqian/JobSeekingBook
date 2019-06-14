# 类

{% tabs %}
{% tab title="例1" %}
```cpp
class A
{
  public:
  A(){ printf(“A”);}
  ~A(){ printf(“~A”);
};
class B:public A
{
  public;
  B(){ printf(“B”);}
  ~B(){ printf(“~B”);}
};
  
int main()
{
  A*c = new B[2];
  delete[] c;
  return 0;
}
```

输出为：ABAB\`~A~A

解析：

删除指向派生类的基类指针时，派生类的析构函数没有被调用，只是调用了基类的析构函数，此时派生类将会导致内存泄漏。解决办法是_**将基类的析构函数声明为虚函数**_，此时在调用析构函数的时候是根据ptr指向的具体类型来调用析构函数，此时会调用派生类的析构函数。
{% endtab %}

{% tab title="Second Tab" %}

{% endtab %}
{% endtabs %}
