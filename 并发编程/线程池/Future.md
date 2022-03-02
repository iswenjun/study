# Future

Future表示异步计算的结果。

提供了一些方法来检查计算是否完成、等待其完成以及检索计算的结果。

~~~java
public interface Future<V> {
    boolean cancel(boolean mayInterruptIfRunning);
    boolean isCancelled();
    boolean isDone();
    V get() throws InterruptedException, ExecutionException;
    V get(long timeout, TimeUnit unit)
        throws InterruptedException, ExecutionException, TimeoutException;
}
~~~



### cancel

试图取消此任务的执行

mayInterruptIfRunning：如果执行此任务的线程应该被中断，则为True;否则，允许完成正在进行的任务



### isCancelled

如果该任务在正常完成之前被取消，则返回true。



### isDone

如果任务完成，返回true。



### get()

如果需要，则等待计算完成，然后检索其结果

如果有必要，最多在给定时间内等待计算完成，然后检索其结果(如果可用)。

timeout：最长等待时间



# FutureTask

FutureTask 实现了 RunnableFuture接口

```java
public class FutureTask<V> implements RunnableFuture<V> {
```

RunnableFuture接口继承了Runnable和Future接口

```java
public interface RunnableFuture<V> extends Runnable, Future<V> {
    void run();
}
```

总结：

FutureTask除了实现了Future接口外（可以通过Future获取线程执行结束的结果），

还实现了Runnable接口（可以直接交给Executor执行）



FutureTask的两种构造方法：

~~~java
    public FutureTask(Callable<V> callable) {
        if (callable == null)
            throw new NullPointerException();
        this.callable = callable;
        this.state = NEW;       // ensure visibility of callable
    }

    public FutureTask(Runnable runnable, V result) {
        this.callable = Executors.callable(runnable, result);
        this.state = NEW;       // ensure visibility of callable
    }
~~~



# 异步计算

Future表示异步计算的结果。提供了一些方法来检查计算是否完成、等待其完成以及检索计算的结果。**结果只能在计算完成时使用get方法进行检索，必要时阻塞直到准备就绪。**取消由cancel方法执行。提供了其他方法来确定任务是正常完成还是被取消。一旦计算完成，计算就不能取消。如果您希望为了可取消性而使用Future但不提供可用的结果，则可以声明Future表单的类型，并作为底层任务的结果返回null。



# CompletabelFuture

