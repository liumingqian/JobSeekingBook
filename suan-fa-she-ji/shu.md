# 树

### 二叉树

{% hint style="info" %}
完全二叉树：除了最后一层，每层都是满的，且最后一层的节点都堆在左边
{% endhint %}

### 哈夫曼树

哈夫曼树的特征是树中只有度为0和度为2的节点

> A:哈夫曼树共有215个节点，对其进行哈夫曼编码可以得到 108 个不同的码字
>
> Q:n0=n2+1 n=n0+n2，解方程得108

红黑树

查找效率的保证

调整时是否会出现无限左旋右旋的情况？

平衡树

红黑树和平衡树的区别

B+树

### 堆

```javascript
var len;    // 因为声明的多个函数都需要数据长度，所以把len设置成为全局变量

function buildMaxHeap(arr) {   // 建立大顶堆
    len = arr.length;
    for (var i = Math.floor(len/2); i >= 0; i--) {
        heapify(arr, i);//自底向上依次调整所有层（表示二叉树的数组的前半截是内部节点，后半截是叶子节点）
    }
}

function heapify(arr, i) {     // 堆调整，调整的是该节点的子树
    var left = 2 * i + 1,
        right = 2 * i + 2,
        largest = i;

    if (left < len && arr[left] > arr[largest]) {
        largest = left;
    }

    if (right < len && arr[right] > arr[largest]) {
        largest = right;
    }

    if (largest != i) {
        swap(arr, i, largest);
        heapify(arr, largest);//从上向下进行调整
    }
}

function swap(arr, i, j) {
    var temp = arr[i];
    arr[i] = arr[j];
    arr[j] = temp;
}

function heapSort(arr) {
    buildMaxHeap(arr);//建堆
    //建堆后堆顶(arr[0])就是最大值
    for (var i = arr.length-1; i > 0; i--) {
        swap(arr, 0, i);//将最大值放到数组末尾，相当于出堆
        len--;
        heapify(arr, 0);//对arr的(0,i)部分再进行堆调整
    }
    return arr;
}
```

