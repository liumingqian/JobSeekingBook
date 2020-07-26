---
description: 关联容器的特点是增加和删除节点对迭代器的影响很小，除了那个操作节点，对其他的节点都没有什么影响。
---

# 关联容器

## Set

红黑树实现。

## Map

map存储结构是[红黑树](../../suan-fa-she-ji/shu.md#hong-hei-shu)，所以需要定义比较函数（less），查找效率为O\(logN\)。对map进行遍历（红黑树的中序遍历）时，输出的结果是有序的。map中节点的构造调用拷贝构造函数。

{% code-tabs %}
{% code-tabs-item title="使用" %}
```cpp
//插入
//如果已经存在键值2015，则会作赋值修改操作，如果没有则插入
ID_Name[2015] = "Tom";
// 插入单个键值对，并返回插入位置和成功标志，插入位置已经存在值时，插入失败
pair<iterator,bool> insert(const value_type& val);//函数声明
mapStudent.insert(map<int, string>::value_type (1, "student_one"));//例子

//在指定位置插入，在不同位置插入效率是不一样的，因为涉及到重排
iterator insert(const_iterator position, const value_type& val);

// 插入多个元素
void insert(InputIterator first, InputIterator last);

//取值
//ID_Name中没有关键字2016，使用[]取值会导致插入
//因此，下面语句不会报错，但打印结果为空
cout<<ID_Name[2016].c_str()<<endl;

//关键字检查
//下面语句会报错
cout<<ID_Name.at(2016);

//查询
//查询关键字为key的元素的个数，在map里结果非0即1
size_t count( const Key& key ) const;

//查询map中键值对的数量
size_t size();

//查找
iterator find();

//迭代器:
begin, end, rbegin,rend 以及对应的const版本cbegin, cend, crbegin,crend

//删除
// 删除迭代器指向位置的键值对，并返回一个指向下一元素的迭代器
iterator erase(iterator pos)

// 删除一定范围内的元素，并返回一个指向下一元素的迭代器
iterator erase(const_iterator first, const_iterator last);

// 根据Key来进行删除， 返回删除的元素数量，在map里结果非0即1
size_t erase(const key_type& key);
```
{% endcode-tabs-item %}
{% endcode-tabs %}

#### 

#### 自定义键值

如果使用了自定义的数据结构作为key，需要提供一个比较函数。map比对键值的时候调用该函数进行判断，如果\(!a&lt;b\)&&\(!b&lt;a\)就会被认为a==b。\(因此相等的元素应当返回false\)

{% code-tabs %}
{% code-tabs-item title="自定义比较函数" %}
```cpp
struct classcomp { 
    bool operator() (const char& lhs, const char& rhs) const 
    {return lhs < rhs;}
};

 map<char,int,classcomp> fourth; 
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% hint style="danger" %}
#### Map中的key不要使用指针

如果key是指针的话，比较的不是指针的内容而是指针地址！
{% endhint %}

## Unordered\_map

unordered\_map底层由HashTable（哈希桶）实现，数据插入和查找的时间复杂度几乎是常数时间，代价是，使用一个比较大的数组来存储元素，形成很多的桶（桶里可能是vector\)，利用hash函数对key进行映射到不同区域进行保存。unordered\_map查找插入的效率优于map。

其取值过程是:

1. 得到key
2. 通过hash函数得到hash值
3. 得到桶号\(一般都为hash值对桶数求模\)
4. 比较桶的内部元素是否与key相等，若都不相等，则没有找到。 
5. 取出key相等的元素的value。

#### 自定义键值

需要实现：

 \(1\) 哈希函数，需要实现一个class重载operator\(\)，将自定义class变量映射到一个size\_t类型的数。一般常用std::hash模板来实现。 

\(2\) 判断两个自定义class类型的变量是否相等的函数，一般在自定义class里重载operator==。

[C++ 之 unordered\_map——哈希表](https://www.acwing.com/blog/content/9/)

```cpp
class Myclass
{
public:
    int first;
    vector<int> second;

    // 重载等号，判断两个Myclass类型的变量是否相等
    bool operator== (const Myclass &other) const
    {
        return first == other.first && second == other.second;
    }
};

// 实现Myclass类的hash函数
namespace std
{
    template <>
    struct hash<Myclass>
    {
        size_t operator()(const Myclass &k) const
        {
            int h = k.first;
            for (auto x : k.second)
            {
                h ^= x;
            }
            return h;
        }
    };
}

//声明
  unordered_map<Myclass, double> S;
```

## hash\_map

不是标准库里的，底层数据结构为hash表，底底层为存放链表的数组

```cpp
#include<ext/hash_map>
```

空间复杂度hash\_map最低，unordered\_map次之，map最大。

#### 哈希表的扩容

根据负载因子决定要不要扩容，扩容时增大存储空间，并把所有元素重新安排进哈希表，因此是非常耗时的操作，程序员应当在开始就分配合适的空间，尽量避免扩容操作。

