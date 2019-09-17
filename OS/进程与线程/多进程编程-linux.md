---
description: >-
  POSIX 在IEEE Std 1003.1c-1995 （也称为POSIX 1995 或 POSIX.1c) 对线程库进行了标准化。开发人员称之为
  POSIX线程，或简称为 Pthreads。Pthreads 是 UNIX 系统上 C 和 C++ 语言的主要线程解决方案。
---

# 多进程/线程编程（Posix）



#### 创建线程

```cpp
#include <pthread.h>
/*
参数：
    thread 指向线程标识符指针。 
    attr 设置线程属性。
    start_routine 线程运行函数起始地址，一旦线程被创建就会执行。 
    arg 运行函数的参数。它必须通过把引用作为指针强制转换为 void 类型进行传递。
        如果没有传递参数，则使用 NULL。
返回值：
    创建线程成功时，函数返回 0，若返回值不为 0 则说明创建线程失败
    */
int pthread_create (thread, attr, start_routine, arg)

//参数依次是：创建的线程id，线程参数，调用的函数，传入的函数参数，返回值为线程id
int ret = pthread_create(&tids[i], NULL, say_hello, NULL);
// 传入的时候必须强制转换为void* 类型，即无类型指针 
rc = pthread_create(&threads[i], NULL,PrintHello, (void *)&(indexes[i]));

void *PrintHello(void *threadid)
{
// 对传入的参数进行强制类型转换，由无类型指针变为整形数指针，然后再读取，传递多个参数可以用struct
int tid = *((int*)threadid);
cout << "Hello Runoob! 线程 ID, " << tid << endl;
pthread_exit(NULL);
}
```

#### 终止线程

{% code-tabs %}
{% code-tabs-item title="End Thread" %}
```cpp
#include <pthread.h>
pthread_exit (status);
```
{% endcode-tabs-item %}
{% endcode-tabs %}

pthread\_exit 用于显式地退出一个线程。通常情况下，pthread\_exit\(\) 函数是在线程完成工作后无需继续存在时被调用。

### 可连接和分离的线程

#### Joinable thread

默认情况下，各个线程间是独立的，一个线程的终止不会通知或影响其他线程，但有时我们需要关心线程终止状态，及释放线程所占的资源。已经终止的线程的资源并不会随着线程的终止而得到释放，我们需要调用 pthread\_join\(\) 来获得另一个线程的终止状态并且释放该线程所占的资源。 默认情况下，创建的线程即是可连接的，这意味着我们可以使用pthread\_join\(\)函数在任何其它线程中等待它（可连接线程）的终止。

```cpp
//此函数将阻塞调用线程，直到目标线程th终止。value_ptr是指向线程th 返回值的指针。
#include <pthread.h>
int pthread_join(
    pthread_t th,  //thread to join
    void **value_ptr   //store value returned by thread
);
```

需要注意的是 th 所表示的线程必须是 joinable 的，一个joinable的线程能够被其他线程收回其资源和杀死；并且只可以有唯一的一个线程对 th 调用 pthread\_join\(\) 。如果 th 处于 detached 状态，那么对 th调用的 pthread\_join\(\)调用将返回错误。

#### Detached thread

detached的线程是不能被其他线程回收或杀死的。如果不关心一个线程的结束状态，那么也可以将一个线程设置为 detached 状态，从而让操作系统在该线程结束时来回收它所占的资源。 将一个线程设置为detached 状态可以通过两种方式来实现。 1. 调用 pthread\_detach\(\) 函数，可以将线程 th 设置为 detached 状态。 2. 创建线程时就将它设置为 detached 状态，首先初始化一个线程属性变量，然后将其设置为 detached 状态，最后将它作为参数传入线程创建函数 pthread\_create\(\)，这样所创建出来的线程就直接处于 detached 状态。

{% hint style="danger" %}
注意：如果设置一个线程为分离线程，而这个线程运行又非常快，它很可能在pthread\_create函数返回之前就终止了，它终止以后就可能将线程号和系统资源移交给其他的线程使用，这样调用pthread\_create的线程就得到了错误的线程号。要避免这种情况可以采取一定的同步措施，最简单的方法之一是可以在被创建的线程里调用pthread\_cond\_timewait函数，让这个线程等待一会儿，留出足够的时间让函数pthread\_create返回。设置一段等待时间
{% endhint %}



```cpp
//创建 detach 线程
pthread_t tid;
pthread_attr_t attr;
pthread_attr_init(&attr);
pthread_attr_setdetachstate(&attr, PTHREAD_CREATE_DETACHED);
pthread_create(&tid, &attr, THREAD_FUNCTION, arg);
```



