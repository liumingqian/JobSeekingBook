# String

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

