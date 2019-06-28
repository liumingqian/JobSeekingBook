# 数学技巧

### 排列组合

![](../.gitbook/assets/image%20%288%29.png)

![](../.gitbook/assets/image%20%2814%29.png)

#### 组合数递归求法

```cpp
//公式：C(n, m) = C(n -1, m - 1) + C(n - 1, m)

int combine(int m,int n)
{
	if(n==m||n==0)
		return 1;
	else
		return combine(m-1,n)+combine(m-1,n-1);
}
```

### 最大公约数/最小公倍数

最大公倍数\(gcd\)通常用最小公约数\(lcm\)求,两个数的最小公约数\*最大公倍数=两数乘积

时间复杂度为O\(logn\)

```cpp
int gcd(int a, int b)
{
    return b ? gcd(b, a % b) : a;
}

```

#### 计算除法上取整

```cpp
int ceil = (x+n-1)/n;
```

#### 循环下标

```cpp
int index=(index+1)%n;
```

[约瑟夫环问题](https://www.cnblogs.com/cmmdc/p/7216726.html)

