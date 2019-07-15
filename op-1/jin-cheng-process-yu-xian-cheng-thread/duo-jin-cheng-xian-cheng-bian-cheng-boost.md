---
description: boost可以开发独立于平台的多线程应用程序
---

# 多进程/线程编程（Windows\)

### 线程安全的定义

* 多线程同时访问时能表现出正确的行为
* 无论操作系统如何调度线程，执行顺序改变后调用方无需额外的同步或协调

### 线程池

线程池在系统启动时即创建大量空闲的线程，称为线程池。程序将一个任务传给线程池，线程池就会启动一条线程来执行这个任务，执行结束以后，该线程并不会死亡，而是再次返回线程池中成为空闲状态，等待执行下一个任务。使用线程池可以很好地提高性能。

设计线程池：

* 设置最大请求数量，避免任务堆积导致的OOM
* 提前创建好若干线程放入池中以减少创建线程的次数
* 考虑是否需要动态修改最大线程数以适应变化的需求

### barrier（同步屏障）

barrier指所有线程都到达同一个节点才能继续往下走。用一个count保存任务数，主线程初始化工作线程后wait这个变量，当工作线程完成工作后对count--，count为0时通知主线程，主线程被唤醒用于合并结果。

### 线程管理

#### 创建线程（boost\)

```cpp
#include <boost/thread/thread.hpp>
#include <iostream>

void hello()
{
      std::cout<<"Hello multi-thread!"<<std::endl;
}

int main(int argc,char* argv[])
{
      boost::thread thrd(&hello);
      thrd.join();//阻塞调用，暂停当前线程，main()函数会一直等待到thrd运行结束。
      return 0;
}
```

上述例子中，thrd被创建后，hello函数就在其所在的线程中被立即执行，同时在main里也并发的执行该函数。

#### 线程结束方式

* join：把指定的线程加入到当前线程，可以将两个交替执行的线程合并为顺序执行的线程。比如在线程B中调用了线程A的Join\(\)方法，直到线程A执行完毕后，才会继续执行线程B。
* detach:是将线程从当前线程分离出去，即不受阻塞

线程启动后，一定要在和线程相关联的thread销毁前调用t.join或t.detach确定以何种方式等待线程执行结束。可以写一个线程类，将t.join或t.detach写在线程类的析构函数里，保证销毁前一定可以调用。

#### 注意线程对局部变量的使用

离开创建线程的代码的作用域后，线程可能还在执行，这是局部变量随着作用域的完成都已销毁，如果线程继续使用局部变量的引用或指针会出现难以排查的错误。所以以detach方式执行线程的时候要用值传递把线程访问的局部数据复制到线程的空间。

**转移线程所有权**

 thread是可移动的，但是不可复制，可以通过move改变线程的所有权，灵活的决定线程在什么时候join或detach。

```text
thread t1(f1);
thread t3(move(t1));
```

这时线程的所有权从t1转移到t3，调用t1.join或t1.detach会出现异常，因此thread也可以作为函数的参数或返回类型。

**线程的标识**

线程的唯一标识可以通过在当前线程调用this\_thread::get\_id\(\)获取，也可以通过thread的实例调用get\_id\(\)获取。

### 并发控制机制

多用户环境中，同一时间可能有多个用户更新相同的记录，因此产生并发性问题。

典型的冲突有：

* **丢失更新**：一个事务的更新覆盖了其他事务的更新结果
* **脏读**：一个事务读取了其他完成一半的事务的记录。

乐观锁和悲观锁是两种并发控制机制，乐观锁减少了加锁的频率，效率高，安全性低。

#### 乐观锁

假定不会发生并发冲突。为数据增加一个版本号，数据每更新一次，对版本号加一，只在提交更新的时候对版本进行比对，如果发现冲突则提交给用户。

或者可以用时间戳实现

#### 悲观锁

假定会发生并发冲突，屏蔽一切可能违反数据完整性的操作，一段逻辑只能有一个线程执行，其他线程在入口处等待，直到锁被释放。

### 互斥锁（mutex）

一个mutex一次只允许一个线程访问共享区，当一个线程想要访问共享区的时候，首先要锁住互斥体。

c++11中有四个互斥对象同于同步多个线程对共享资源的访问。

* mutex：最基本的互斥对象
* timed\_mutex:带有超时机制的互斥对象，允许等待一段时间，超时后仍未获得互斥对象的所有权时放弃等待。
* recursive\_mutex:递归互斥锁，可以被同一个线程多次加锁，以获得对互斥锁对象的多层所有权。当获取锁的函数间有相互调用的关系，如果使用非递归锁就会立即死锁。
* recursive\_timed\_mutex

```cpp
MutexLock mutex;  

void foo()  
{  
    mutex.lock();  
    // do something  
    mutex.unlock();  
}  

void bar()  
{  
    mutex.lock();  
    // do something  
    foo();  //死锁
    mutex.unlock();   
}  
```

ref：[https://blog.csdn.net/m18718300471/article/details/79927948](https://blog.csdn.net/m18718300471/article/details/79927948)

c++还提供了两种锁类型：

* lock\_guard:严格基于作用域的锁管理类模板，构造是是否加锁是可选的（不加锁时假定当前线程已获得锁的所有权）析构时自动释放锁，所有权不可转移。对象生存期内不允许手动加锁和释放锁。
* unique\_lock:更加灵活，构造时是否加锁是可选的，对象析构时如果持有锁会自动释放锁。所有权可以转移，生命周期内允许手动加锁和释放锁。

#### 死锁：

*  如果提前中抛出异常或return，而导致没有解锁就退出，就会发生死锁。

使用RAII（资源分配时初始化）方法管理互斥对象可以避免死锁。boost设计了scoped\_lock模式，在构造和析构函数中对mutex加锁或解锁，c++语言规范保证，即使有异常抛出也会调用析构函数。 scoped\_lock是一种unique\_lock;

**加锁策略：** 构造时是否加锁是可选的，其加锁策略有：

* defer\_lock:不请求锁
* try\_to\_lock:尝试请求锁但不阻塞线程，锁不可用时也会立即返回
* adopt\_lock:假定当前线程已经获得互斥对象的所有权，所以不再请求锁。

lock\_guard使用的是第三种策略。

{% code-tabs %}
{% code-tabs-item title="Boost " %}
```cpp
#include <boost/thread/thread.hpp>
#include <boost/thread/mutex.hpp>
#include <boost/thread/bind.hpp>

#include <iostream>

boost::mutex io_mutex;

void count(int id)
{
      for(int i = 0;i < 10; i++)
      {
            boost::mutex::scoped_lock lock(io_mutex);
            std::cout<<id<<":"<<i<<std::endl;
      }
}

int main(int argc,char* argv[])
{
      boost::thread thrd1(boost::bind(&count,1));
      boost::thread thrd2(boost::bind(&count,2));
      thrd1.join();
      thrd2.join();
      return 0;
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

**读写锁的实现**

```cpp
typedef boost::shared_lock<boost::shared_mutex> readLock;
typedef boost::unique_lock<boost::shared_mutex> writeLock;
boost::shared_mutex rwmutex;

void readOnly()
{
  readLock rdlock(rwmutex);
  // do something
}

void writeOnly()
{
  writeLock wtlock(rwmutex);
  // do something
}
```

read\_lock是共享锁：对同一个rwmutex，线程可以同时有多个readLock，这些readLock会阻塞任意一个企图获得writeLock的线程，直到所有的readLock对象都析构。

write\_lock是独占锁：如果writeLock首先获得了rwmutex，那么它会阻塞任意一个企图在rwmutex上获得readLock或者writeLock的线程。

#### 条件变量

通常与互斥量一起使用，用来等待。

通常情况是一个线程锁住一个互斥量，当它不能获得它期待的结果时，等待一个条件变量，直到另一个线程通知它条件满足了，唤醒该线程继续执行。



### 自旋锁

不会引起调用者在内核态和用户态之间切换。调用者轮询（消耗CPU时间）检查该自旋锁的保持者是否已经释放锁。

#### Fork

* fork复制一个与父进程完全相同的子进程，共享代码空间，但有独立的数据空间。子进程数据空间是父进程的完整拷贝，运行到的位置也与父进程相同。
* 该函数被调用一次但返回两次，子进程返回值是0，父进程返回值是子进程的进程id。

