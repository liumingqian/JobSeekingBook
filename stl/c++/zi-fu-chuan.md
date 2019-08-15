---
description: 'https://blog.csdn.net/v_JULY_v/article/details/6417600'
---

# 字符串

### 初始化

```cpp
char *test = "error"; //错误：右值类型为const char[6]
```

### 求长度

size\(\)是string的一个函数（不是sizeof!\)，返回值不包括字符串结尾\0。

strlen返回值不包括字符串结尾\0。

### 比较

两个字符串自左向右逐个字符相比（按ASCII值大小相比较），直到出现不同的字符或遇’\0’为止。如：

“A”&lt;”B” 

“a”&gt;”A” 

“computer”&gt;”compare”



### 库函数实现

```cpp

char * strcpy( char *strDest, const char *strSrc )      //对输入加const
{     
	if(strDest == strSrc) { return strDest; }  //判重
	assert( (strDest != NULL) && (strSrc != NULL) ); //判空    
	char *address = strDest;      
	while( (*strDest++ = * strSrc++) != '\0' );      
	return address;     //返回目的地址，实现链式操作
}


int strcmp(const char *s, const char *t)   
{   
    assert(s != NULL && t != NULL);   
    while (*s && *t && *s == *t)   
    {   
        ++ s;   
        ++ t;   
    }   
    return (*s - *t);   
}  
```

