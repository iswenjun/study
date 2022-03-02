# ThreadPoolExecutor

## 构造方法

~~~java
    public ThreadPoolExecutor(int corePoolSize,
                              int maximumPoolSize,
                              long keepAliveTime,
                              TimeUnit unit,
                              BlockingQueue<Runnable> workQueue,
                              ThreadFactory threadFactory,
                              RejectedExecutionHandler handler) {

    }
~~~



# 七大参数

### corePoolSize

the number of threads to keep in the pool, even if they are idle, unless allowCoreThreadTimeOut is set

线程池中保留的线程数，即使它们是空闲的，除非设置allowCoreThreadTimeOut



### maximumPoolSize

the maximum number of threads to allow in the pool

池中允许的最大线程数



### keepAliveTime

when the number of threads is greater than the core, this is the maximum time that excess idle threads will wait for new tasks before terminating.

当线程数大于核心数时，这是剩余空闲线程在终止前等待新任务的最大时间。



### unit

the time unit for the keepAliveTime argument



### workQueue

the queue to use for holding tasks before they are executed. 

This queue will hold only the Runnable tasks submitted by the execute method.

在任务执行之前用来保存任务的队列。

这个队列将只保存由execute方法提交的Runnable任务。



### threadFactory

the factory to use when the executor creates a new thread

执行器创建新线程时要使用的工厂



### handler

the handler to use when execution is blocked because the thread bounds and queue capacities are reached

当执行因线程边界和队列容量达到而被阻塞时使用的处理程序



# 任务提交和任务执行

execute和submit



## execute

* execute只能执行Runnable类型的任务，无返回值
* 执行任务时，遇到异常会直接抛出

~~~java
    // Executes the given task sometime in the future.
	public void execute(Runnable command) {
    }
~~~



## submit

* 可以提交Runnable类型的任务，返回值为null
* 也可以提交Callable类型的任务，会有一个Future类型的返回值
* 不会直接抛出异常，只有在使用Future的get方法获取返回值时，才会抛出异常

~~~java
    public Future<?> submit(Runnable task) {
    }

    public <T> Future<T> submit(Runnable task, T result) {
    }

    public <T> Future<T> submit(Callable<T> task) {
    }
~~~



Waits if necessary for the computation to complete, and then retrieves its result.
Returns:	the computed result
Throws:
CancellationException – if the computation was cancelled
ExecutionException – if the computation threw an exception
InterruptedException – if the current thread was interrupted while waiting

~~~java
	V get() throws InterruptedException, ExecutionException;
~~~



# 线程池参数调节

CPU密集型（计算密集型）：corePoolSize = CPU核数 + 1

IO密集型：corePoolSize = CPU核数 * 2



# 线程池监控

* getActiveCount：返回活动执行任务的大约线程数。
* getCompletedTaskCount()：返回已完成执行的任务的大致总数。
* getCorePoolSize()：返回核心线程数。
* getLargestPoolSize()：返回池中同时存在的最大线程数。
* getMaximumPoolSize()：返回允许的最大线程数。
* getPoolSize()：返回当前线程池中的线程数。
* getTaskCount()：返回计划执行的任务的大致总数。

