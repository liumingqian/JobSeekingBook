# 数学技巧

### 排列组合

![](../.gitbook/assets/image%20%281%29.png)

![](../.gitbook/assets/image%20%282%29.png)

#### 组合递归公式

```cpp
//C(n, m) = C(n -1, m - 1) + C(n - 1, m)
C[1][0] = C[1][1] = 1;
	for (int i = 2; i < N; i++){
		C[i][0] = 1;
		for (int j = 1; j < N; j++)
			C[i][j] = (C[i - 1][j] + C[i - 1][j - 1]);
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

