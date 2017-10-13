# java队列

## 队列概览 {#队列概览}

[![](http://sqtds.github.io/img/2015/Queue.png)](http://sqtds.github.io/img/2015/Queue.png)

### 1，BlockingQueue {#1，BlockingQueue}

* ArrayBlockingQueue，数组结构组成,其构造函数必须带一个int参数来指明其大小
* LinkedBlockingQueue，由链表结构组成,若其构造函数带一个规定大小的参数，生成的BlockingQueue有大小限制，若不带大小参数，所生成的BlockingQueue的大小由Integer.MAX\_VALUE来决定
* PriorityBlockingQueue，其所含对象的排序不是FIFO,而是依据对象的自然排序顺序或者是构造函数的Comparator决定的顺序
* DelayQueue：一个使用优先级队列实现的无界阻塞队列。DelayQueue是一个支持延时获取元素的无界阻塞队列。队列使用PriorityQueue来实现。队列中的元素必须实现Delayed接口，**在创建元素时可以指定多久才能从队列中获取当前元素**。**只有在延迟期满时才能从队列中提取元素。**我们可以将DelayQueue运用在以下应用场景：

  缓存系统的设计：  
  可以用DelayQueue保存缓存元素的有效期，使用一个线程循环查询DelayQueue，一旦能从DelayQueue中获取元素时，表示缓存有效期到了。  
  定时任务调度：  
  使用DelayQueue保存当天将会执行的任务和执行时间，一旦从DelayQueue中获取到任务就开始执行，从比如TimerQueue就是使用DelayQueue实现的。

* SynchronousQueue：一个不存储元素的阻塞队列。每一个put操作必须等待一个take操作，否则不能继续添加元素。

* LinkedTransferQueue：一个由链表结构组成的无界阻塞队列。
* LinkedBlockingDeque：一个由链表结构组成的双向阻塞队列。

### 2，Queue {#2，Queue}

* PriorityQueue是个基于优先级堆的极大优先级队列。  
  此队列按照在构造时所指定的顺序对元素排序，既可以根据元素的自然顺序来指定排序（参阅 Comparable）， 也可以根据 Comparator 来指定，这取决于使用哪种构造方法。优先级队列不允许 null 元素。

* ConcurrentLinkedQueue一个无锁的并发线程安全的队列。对比锁机制的实现，使用无锁机制的难点在于要充分考虑线程间的协调。简单的说就是多个线程对内部数据结构进行访问时，如果其中一个线程执行的中途因为一些原因出现故障，其他的线程能够检测并帮助完成剩下的操作。这就需要把对数据结构的操作过程精细的划分成多个状态或阶段，考虑每个阶段或状态多线程访问会出现的情况。  
  需要说一下的是，**ConcurrentLinkedQueue的size\(\)是要遍历一遍集合的，所以尽量要避免用size而改用isEmpty\(\)，以免性能过慢。**

### 3，Deque {#3，Deque}

LinkedList，ArrayDeque，非线程安全的，可以通过Collections的工具方法改造成线程安全的。  
LinkedBlockingDeque

按照我们一般的理解，Deque是一个双向队列，这将意味着它不过是对Queue接口的增强。如果仔细分析Deque接口代码的话，我们会发现它里面主要包含有4个部分的功能定义。

1. 双向队列特定方法定义。
2. Queue方法定义。
3. **Stack方法定义。**
4. Collection方法定义。

```
\\Stack定义
    public void push(E e) {
        addFirst(e);
    }
    public E pop() {
        return removeFirst();
    }
 	public E peek();

```

### 4,参考资料 {#4,参考资料}

[聊聊并发（七）——Java中的阻塞队列](http://www.infoq.com/cn/articles/java-blocking-queue)  
[Java多线程（五）之BlockingQueue深入分析](http://blog.csdn.net/guangcigeyun/article/details/8278350)  
[Java多线程（六）之Deque与LinkedBlockingDeque深入分析](http://blog.csdn.net/guangcigeyun/article/details/8278355)

  




