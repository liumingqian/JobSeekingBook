# 内存管理

### 

### 逻辑地址/物理地址和虚拟内存。

* 逻辑地址：CPU所生成的地址，计算机用户（开发人员）看到的地址。如，创建数组时，操作系统会返回一个逻辑上的连续空间。但逻辑地址和元素存储的真实地址不相干，是操作系统通过地址映射映射成连续的的，是相对于你当前进程数据段的地址（偏移地址）。
* 物理地址：加载到内存中的地址，内存单元的真正地址，在总线上传输使用物理地址，不必转换，不必分页。
* 虚拟内存：程序在物理地址中寻址的范围与机器字长有关，在32位平台下，寻址的范围是2^32=4G。虚拟内存的好处：如果没有虚拟内存，每次开启一个进程都分配4G物理内存给它就很没效率，并且我们不希望程序指令可以直接访问物理内存。因此每个进程运行的时候会得到4G虚拟内存，但实际虚拟内存可能只对应一点物理内存，实际用了多少内存就会对应多少物理内存。从进程的角度看，它拥有4G连续的地址空间，实际上是被分隔成多个物理内存碎片，还有一部分碎片存储在外部磁盘，在需要时进行数据交换。这样物理内存就不需要是连续的了。

> 当进程访问地址时发生了什么？
>
> 1、将逻辑地址翻译为实际的物理内存地址
>
> 2、进程只把自己当前需要的虚拟地址空间映射到物理内存上，并通过页表来记录哪些地址空间上的数据是在物理内存上，哪些实际上存在磁盘上。
>
> 3、当进程访问虚拟地址的时候会先看页表，如果发现对应的数据不在物理内存上，就会发生缺页异常。
>
> 4、缺页异常的处理方法是，操作系统阻塞该进程，并将硬盘里对应的页换入内存，并使该进程就绪。如果内存已经满了，根据页面置换算法找到一个页进行覆盖。

### 分页内存管理

使进程的物理地址空间可以是非连续的。

物理内存被划分为很多小块，每块被称为帧，分配内存时帧是最小单位。逻辑内存中与帧对应的概念是页。
