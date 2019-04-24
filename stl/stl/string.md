---
description: 'ref: https://blog.csdn.net/u013834525/article/details/82533935'
---

# String

### 使用

{% code-tabs %}
{% code-tabs-item title="从字符串和字符数组构造" %}
```cpp
int main()
{
	const char* str1 = "attr";
	string str2("attr");
	string str3(str1);
	const char* str4 = 
"attr";
 ////str1和str4一样，其他两个都不一样，常量字符串“attr”保存在全局常量区
	cout << "str1的地址:" << (void*)str1 << endl;
	cout << "str2的地址:" << (void*)str2.c_str() << endl;
	cout << "str3的地址:" << (void*)str3.c_str() << endl;
	cout << "str4的地址:" << (void*)str4 << endl;
	return 0;
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% code-tabs %}
{% code-tabs-item title="常用功能" %}
```cpp
//截取子串
string substr(size_t pos = 0, size_t len = npos);

//合并
//注意参数和返回值都是引用！
string& append(const string& str);

//IO
//参数：std::cin,str,终结符，遇到该字符停止操作
istream& getline(istream &is , string &str , char delim );
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% code-tabs %}
{% code-tabs-item title="algorithm" %}
```cpp
#include <algorithm> 
//查找
//find_first_of 函数和find函数最大的区别就是如果在一个字符串str1中查找另一个字符串str2，
//如果str1中含有str2中的任何字符，则就会查找成功，而find则不同；
//同族函数还有rfind、find_first_of、find_last_of
//find_first_not_of、find_last_not_of
//**注意**：返回值为size_t(unsigned int)
size_t find(const string& str, size_t pos = 0);
size_t find(const char* s, size_t pos = 0);
size_t find(const char* s, size_t pos, size_type n);//之比较前n个字符

//反转
std::reverse(str.begin(),str.end());
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% code-tabs %}
{% code-tabs-item title="大小写转换" %}
```cpp
#include <ctype.h>：

int tolower ( int c );
int toupper ( int c );
```
{% endcode-tabs-item %}
{% endcode-tabs %}

### 与其他类型的转换 

1、格式化字符串

```cpp
 //将data转换为字符串
 sprintf(str,"%d",data);
 //连接字符串s1和s2
 sprintf(str,"%s %s",s1,s2);
 
  char s[15] = "123.432,432";
  int n;
  double f1;
  int f2;
  sscanf(s, "%lf,%d%n", &f1, &f2, &n);
```

### stringStream

```cpp
#include <sstream>

stringstream ss;//默认构造函数，从当前流中内容构建stringstream
stringstream ss(str);//从字符串str构造stringstream

ss.str(s);//用字符串s填充ss

ss << N;//向ss流中输入值
ss >> str;//向str中写入

ss.clear();//清除流中内容
```

