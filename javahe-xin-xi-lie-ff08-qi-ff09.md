# java线程

## thread {#thread}

[![](http://sqtds.github.io/img/2015/thread.png)](http://sqtds.github.io/img/2015/thread.png)

### 1，锁 {#1，锁}

偏向锁\(Biased Locking\)是Java6引入的一项多线程优化。它通过消除资源无竞争情况下的同步原语，进一步提高了程序的运行性能。

轻量级锁（Lightweight Locking）本意是为了减少多线程进入互斥的几率，并不是要替代互斥。

轻量级锁也是一种多线程优化，它与偏向锁的区别在于，轻量级锁是通过CAS来避免进入开销较大的互斥操作，而偏向锁是在无竞争场景下完全消除同步，连CAS也不执行（CAS本身仍旧是一种操作系统同步原语，始终要在JVM与OS之间来回，有一定的开销）。

### 2，同步 {#2，同步}

wait:方法使当前线程暂停执行并释放对象锁标示，让其他线程可以进入synchronized数据块，当前线程被放入对象等待池中。

notify:将从对象的等待池中移走一个任意的线程并放到锁标志等待池中，只有锁标志等待池中线程能够获取锁标志；如果锁标志等待池中没有线程，则notify\(\)不起作用。

notifyAll:从对象等待池中移走所有等待那个对象的线程并放到锁标志等待池中。

**注意 这三个方法都是java.lang.Object的方法,调用时需要获取锁**

### 3，实例方法 {#3，实例方法}

join:主要是让调用改方法的thread完成run方法里面的东西后， 在执行join\(\)方法后面的代码。

```
t1.start();  
t1.join(); 
// wait t1 to be finished  

t2.start();  
t2.join(); 
// in this program, this may be removed
```

interupt:中断是一种协作机制。当一个线程中断另一个线程时，被中断的线程不一定要立即停止正在做的事情。相反，中断是礼貌地请求另一个线程在它愿意并且方便的时候停止它正在做的事情。

### 4，静态方法 {#4，静态方法}

sleep：在指定的毫秒数内让当前正在执行的线程休眠（暂停执行），此操作受到系统计时器和调度程序精度和准确性的影响。该线程不丢失任何监视器的所属权。 通过调用sleep使任务进入休眠状态，在这种情况下，任务在指定的时间内不会运行。调用sleep的时候锁并没有被释放。

yield：线程调用yield\(\)方法后，表明自己做的事已经完成，让出自己的cpu时间给其他线程使用。让出后，该线程可以重新获得cpu分配的权利，状态变为了可执行状态。（yield并不意味着退出和暂停，只是，告诉线程调度如果有人需要，可以先拿去，我过会再执行，没人需要，我继续执行）调用yield的时候锁并没有被释放。

### 5，参考资料 {#5，参考资料}

* [java中的Synchronized 实现](http://blog.csdn.net/hsuxu/article/details/9472371)
* [Java 理论与实践: 处理 InterruptedException](http://www.ibm.com/developerworks/cn/java/j-jtp05236.html)
* [Java并发结构](http://ifeve.com/java-concurrency-constructs/)



