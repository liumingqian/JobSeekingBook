#二分

### 题型

\* 矩阵型

\* 线型

https://www.cnblogs.com/yedushusheng/p/5524166.html
```cpp
//两个模板的区别在于
int bsearch_1(int l, int r)
{
    while (l < r)
    {
        int mid = l + r >> 1;
        if (check(mid)) r = mid;
        else l = mid + 1;
    }
    return l;
}
int bsearch_2(int l, int r)
{
    while (l < r)
    {
        int mid = l + r + 1 >> 1;
        if (check(mid)) l = mid;
        else r = mid - 1;
    }
    return l;
}
```

### 单调栈和单调队列
在线性时间内解决一定范围内的数的问题。
例题：最右侧的第一个较大的数，窗口内较大的数
