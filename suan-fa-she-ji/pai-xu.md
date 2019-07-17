# 排序

### 八大排序复杂度比较

![](../.gitbook/assets/image%20%288%29.png)



### 快速排序

[复杂度递推公式](suan-fa-fu-za-du-fen-xi.md#kuai-pai)

#### 基准选择

* 选择第一个或最后一个元素：如果数组是排好序的，这种选法会导致O（n^2）的复杂度
* 选择随机数：安全但生成随机数是昂贵的算法
* 选择left和right的中值：比较好

```cpp
int partition(int *l, int left, int right)
{
	int tmp = l[right];
	while (left != right)
	{
		while (left < right&&l[left] <= tmp)
			left++;
		if (left < right)
		{
			l[right] = l[left];
			right--;
		}
			
		while (left < right&&l[right] >= tmp)//这里不是left<=right!
			right--;
		if (left < right)//还有这里
		{
			l[left] = l[right];
			left++;
		}
			
	}
	l[left] = tmp;
	return left;
}

void QuickSort(int*l, int left, int right)
{
	if (left >= right)//leftright不合理返回
		return;
	int pivot = (left + right) / 2;//确定轴值并与最右交换
	swap(l, right, pivot);
	pivot = partition(l, left, right);//用partition函数把序列按轴值分成两块
	QuickSort(l, left, pivot-1);//对这两块分别再quicksort
	QuickSort(l, pivot+1, right);
}
```

### 树形排序-锦标赛算法

需要2n的额外空间来表示一颗树，首先将待查找的所有数据拷贝到2n空间的末尾，作为树的叶子节点，两两比较叶子节点，将胜者依次放到n/2的节点。

![](../.gitbook/assets/image%20%2881%29.png)

选出最小值后，将叶子节点中的13改为INF，并重新从叶子节点的13的位置出发，更新树。

![](../.gitbook/assets/image%20%2828%29.png)

堆排序是树形排序的优化。

### 堆排序

堆是一种父子节点间保持大小关系的完全二叉树

```cpp
/*小根堆调整*/  
void MinHeapify(int * array, int  heapSize, int  currentNode)  
{  
    int  leftChild, rightChild,  minimum;  //左、右孩子下标；三者最小元素下标
    leftChild = 2*currentNode + 1;  //当前结点的做左孩子
    rightChild = 2*currentNode + 2; //当前结点的做右孩子 
    if(leftChild < heapSize && array[leftChild] < array[currentNode])  
        minimum = leftChild;  //有左孩子并且左孩子较小
    else  
        minimum = currentNode;  
    if(rightChild < heapSize && array[rightChild] < array[minimum])  
        minimum = rightChild; //有右孩子并且右孩子是三者最小的 
    if(minimum != currentNode)  
    {//    当前结点不是最小的，需要交换
        Swap(array, minimum, currentNode);  
        MinHeapify(array, heapSize, minimum);  //子节点需要重新递归调整
    }  
}  
/*构建小根堆*/  
void MinHeapCreate(int * array, int  heapSize)  
{  
    int i;  
    for(i = heapSize/2; i >= 0; i--)  
    { //需要从最后一个非叶节点开始调整
        MinHeapify(array, heapSize, i);  
    }  
}  
```

### TopK问题

1.冒泡n次 

2.堆排序 

3.快排partition思想，返回pivot跟k进行比较，并递归的缩小区间直到pivot=k

