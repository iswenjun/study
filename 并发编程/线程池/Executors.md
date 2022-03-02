# Executors

> Factory and utility methods for Executor, ExecutorService, ScheduledExecutorService, ThreadFactory, and Callable classes defined in this package.
>
> 这个包中定义了Executor、ExecutorService、ScheduledExecutorService、ThreadFactory和Callable类的工厂和实用方法。

通过Executors可以创建特定功能的线程池（ThreadPoolExecutor）

~~~java
        // 创建一个固定数量的线程池
        ExecutorService executorService1 = Executors.newFixedThreadPool(10);
        // 创建一个单线程的线程池
        ExecutorService executorService3 = Executors.newSingleThreadExecutor();
        // 创建一个线程池，该线程池根据需要创建新线程，但将在可用时重用以前构造的线程。
        ExecutorService executorService4 = Executors.newCachedThreadPool();
        // 创建一个线程池，该线程池可以调度命令在给定的延迟后运行或定期执行。
        ScheduledExecutorService executorService = Executors.newScheduledThreadPool(10);
~~~



## FixThreadPool

一个固定数量的线程池，该线程池的线程数量始终不变

有任务提交时，若有空闲线程，则立即执行，若没有，则会被缓存到一个任务队列中等待有空闲的线程

核心线程数等于最大线程数，keepAliveTime为0

~~~java
    public static ExecutorService newFixedThreadPool(int nThreads) {
        return new ThreadPoolExecutor(nThreads, nThreads,
                                      0L, TimeUnit.MILLISECONDS,
                                      new LinkedBlockingQueue<Runnable>());
    }

    public static ExecutorService newFixedThreadPool(int nThreads, ThreadFactory threadFactory) {
        return new ThreadPoolExecutor(nThreads, nThreads,
                                      0L, TimeUnit.MILLISECONDS,
                                      new LinkedBlockingQueue<Runnable>(),
                                      threadFactory);
    }
~~~





## CachedThreadPool

一个可根据实际情况调整线程个数的线程池，不限制最大线程数量。

若有任务而没有线程时会创建线程，每个线程空闲等待时间为60秒，60秒后如果该线程没有任务可执行，将被回收

corePoolSize为0，maximumPoolSize为最大，keepAliveTime为60秒

~~~java
    public static ExecutorService newCachedThreadPool() {
        return new ThreadPoolExecutor(0, Integer.MAX_VALUE,
                                      60L, TimeUnit.SECONDS,
                                      new SynchronousQueue<Runnable>());
    }

    public static ExecutorService newCachedThreadPool(ThreadFactory threadFactory) {
        return new ThreadPoolExecutor(0, Integer.MAX_VALUE,
                                      60L, TimeUnit.SECONDS,
                                      new SynchronousQueue<Runnable>(),
                                      threadFactory);
    }
~~~



## SingleThreadExecutor

创建一个线程数量为1的线程池，若空闲则执行，否则放入队列等待执行

核心线程数为1，最大线程数也为1，keepAliveTime为0

~~~java
    public static ExecutorService newSingleThreadExecutor() {
        return new FinalizableDelegatedExecutorService
            (new ThreadPoolExecutor(1, 1,
                                    0L, TimeUnit.MILLISECONDS,
                                    new LinkedBlockingQueue<Runnable>()));
    }

    public static ExecutorService newSingleThreadExecutor(ThreadFactory threadFactory) {
        return new FinalizableDelegatedExecutorService
            (new ThreadPoolExecutor(1, 1,
                                    0L, TimeUnit.MILLISECONDS,
                                    new LinkedBlockingQueue<Runnable>(),
                                    threadFactory));
    }
~~~



## ScheduledThreadPool

指定核心线程数量，返回一个ScheduledThreadPoolExecutor

DelayedWorkQueue：专门的延迟队列

~~~java
    public static ScheduledExecutorService newScheduledThreadPool(int corePoolSize) {
        return new ScheduledThreadPoolExecutor(corePoolSize);
    }
    
    public static ScheduledExecutorService newScheduledThreadPool(
            int corePoolSize, ThreadFactory threadFactory) {
        return new ScheduledThreadPoolExecutor(corePoolSize, threadFactory);
    }
	// ScheduledThreadPoolExecutor构造方法
    public ScheduledThreadPoolExecutor(int corePoolSize) {
        super(corePoolSize, Integer.MAX_VALUE, 0, NANOSECONDS,
              new DelayedWorkQueue());
    }
~~~



## defaultThreadFactory()

以上方法未指定线程工厂时，会使用Executors的此方法返回一个默认的线程工厂

```java
    // Returns a default thread factory used to create new threads
	public static ThreadFactory defaultThreadFactory() {
        return new DefaultThreadFactory();
    }
```
