# ConcurrentHashMap

# 对比

## 与HashMap的区别

ConcurrentHashMap是线程安全的

HashMap不是线程安全的

## 与HashTable的区别

HashTable也是线程安全的，但每次操作的时候都要**锁住整个结构**

Hashtable是快速失败的，遍历时改变结构会报错ConcurrentModificationException。

ConcurrentHashMap是安全失败，允许并发检索和更新。

## JDK8和7的ConcurrentHashMap的区别

* 8中新增了红黑树
* 7使用的是头插法，8使用的是尾插法
* 7使用了分段锁，8没有使用分段锁
* 7使用了ReentrantLock，而8使用的是Synchronized
* 7中的扩容是每个Segment内部进行扩容，不会影响其他Segment。而ConcurrentHashMap与hashmap扩容类似，只是支持了多线程扩容，保证了线程安全。

# 特性

## ConcurrentHashMap是如何保证并发安全的

JDK7：ReentrantLock+CAS+Segment

在JDK7的ConcurrentHashMap中，有一个Segment数组，Segment相当于一个小的HashMap。

Segment继承了ReentrantLock类，而且提供put，get方法

JDK8：synchronized+cas

有一个Node数组，Node包含key、value、hashcode信息，和HashMap中的Entry一样。

通过对Node数组的某个index位置的元素进行同步，达到该index位置的并发安全。

同时内部也利用了CAS对数组的某个位置进行并发安全的赋值。

## JDK8中的ConcurrentHashMap为什么使用synchronized来进行加锁？

JDK8中使用synchronized加锁时，是对链表头结点和红黑树根结点来加锁的，而ConcurrentHashMap会保证，数组中某个位置的元素一定是链表的头结点或红黑树的根结点，所以JDK8中的ConcurrentHashMap在对某个桶进行并发安全控制时，只需要使用synchronized对当前那个位置的数组上的元素进行加锁即可，对于每个桶，只有获取到了第一个元素上的锁，才能操作这个桶，不管这个桶是一个链表还是红黑树。

## JDK8中的ConcurrentHashMap是如何扩容的

[ConcurrentHashMap（JDK8） - 云+社区 - 腾讯云 (tencent.com)](https://cloud.tencent.com/developer/article/1873182)

