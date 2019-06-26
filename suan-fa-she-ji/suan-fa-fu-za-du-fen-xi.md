# 算法复杂度分析

### 排序

#### 快排递推公式

T（n）≤2T（n/2） +n，T（1）=0  
T（n）≤2（2T（n/4）+n/2） +n=4T（n/4）+2n  
T（n）≤4（2T（n/8）+n/4） +2n=8T（n/8）+3n  
……  
T（n）≤nT（1）+（log2n）×n= O\(nlogn\)

![](../.gitbook/assets/image%20%2837%29.png)

