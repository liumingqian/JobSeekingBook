# 高级特性

## STL ：Allocator

new 一个对象的时候先调用::operator new 分配一个对象大小的内存，然后在这个内存上调用FOO::FOO\(\)构造对象。同样，当你delete 一个对象的时候先调用FOO::~FOO\(\) 析构掉对象，再调用::operator delete将对象所处的内存释放。为了精密分工，STL 将allocator决定将这两个阶段分开。分别用 4 个函数来实现：

　　1.内存的配置：alloc::allocate\(\);

　　2.对象的构造：::construct\(\);

　　3.对象的析构：::destroy\(\);

　　4.内存的释放：alloc::deallocate\(\);

假设你有一个特殊的 fast\_allocator，能快速分配和释放内存（也许通过放弃线程安全性，或使用一个小的局部堆）

