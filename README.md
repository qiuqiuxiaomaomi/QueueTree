# QueueTree
队列技术研究

Java的内置队列

![](https://i.imgur.com/xrJDeBT.png)

<pre>
     队列的底层一般分为3种：
         1）数组
         2）链表
         3）堆
     堆一般情况下是为了实现带有优先级特性的队列。

     ArrayBlockingQueue通过加锁的方式保证线程安全
     LinkedBlockQueue通过锁的方式实现线程安全
     ConcurrentLinkedQueue以及LinkedTransferQueue则是通过原子变量compare and swap,
          CAS这种不加锁的方式来实现的。

     通过不加锁的方式实现的队列都是无界的，无法保证队列的长度在确定的范围内；
     而加锁的方式，可以实现有界队列。
     在稳定性要求特别高的系统中，为了防止生产者速度过快，导致内存溢出，只能选择有界队列；
     同时为了减少Java的垃圾回收队系统性能的影响，会尽量选择array/heap格式的数据结构，
     这样符合条件的队列筛选下来只有ArrayBlockingQueue;
</pre>

<pre>

</pre>