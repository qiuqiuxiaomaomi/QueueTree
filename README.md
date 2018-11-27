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

![](https://i.imgur.com/XwfvaLV.png)

<pre>
ArrayBlockingQueue在实际使用的过程中，因为加锁和伪共享等出现严重的性能问题。

     1）加锁
        现实编程过程中，加锁通常会严重的影响性能。线程会因为竞争不到锁而被挂起，等锁被释放
        的时候，线程又会被恢复，这个过程中存在很大的开销，并且通常会有较长时间的中断，因为
        当一个线程正在等待锁时，它不能做任何其他事情。如果一个线程在持有锁的情况下被延迟
        执行，例如发生了缺页中断，调度延迟或者其他类似情况，那么所有需要这个锁的线程都无法
        执行下去。如果被阻塞的线程的优先级较高，而持有锁的线程优先级较低，就会发生优先级
        反转。

        Distruptor论文中讲述了一个实验：
            .这个测试程序调用了一个函数，该函数会对一个64位的计数器循环自增5亿次
            .机器环境： 2.4G 6核
            .运算：64位的计算器累加5亿次

        CAS操作比单线程无锁慢了1个数量级；
        有锁且多线程并发的情况下，速度比单线程无锁情况下慢3个数量级；
        
        单线程情况下，不加锁的性能 > CAS操作的性能 > 加锁的性能

        在多线程情况下，为了保证线程安全，必须使用CAS或锁，这种情况下，CAS的性能超过锁的
        性能，前者大约是后者的8倍。
</pre>

<pre>
关于锁与CAS
</pre>