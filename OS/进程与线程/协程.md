# 协程

![](../../.gitbook/assets/image%20%2884%29.png)

协程在一个线程内部执行，比线程更轻量级，不由操作系统内核管理，由程序所管理，在用户态执行，没有线程切换的资源消耗，也不会存在线程间同步那样的问题。

### 协程的实现

c/c++原生不支持协程，有boost.context,libco等协程库。

表示协程的数据结构例如：

{% code-tabs %}
{% code-tabs-item title="libco中的coroutine数据结构" %}
```cpp
struct stCoRoutine_t
{
    stCoRoutineEnv_t *env;  // 协程运行环境
    pfn_co_routine_t pfn;   // 协程执行的逻辑函数
    void *arg;              // 函数参数
    coctx_t ctx;            // 保存协程的下文环境 
    ...
    char cEnableSysHook;    // 是否运行系统 hook，即非侵入式逻辑
    char cIsShareStack;     // 是否在共享栈模式
    void *pvEnv;
    stStackMem_t* stack_mem;  // 协程运行时的栈空间
    char* stack_sp;           // 用来保存协程运行时的栈空间
    unsigned int save_size;
    char* save_buffer;
};
```
{% endcode-tabs-item %}
{% endcode-tabs %}

