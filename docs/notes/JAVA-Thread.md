# Java 多线程

### 一、线程基础

#### 1. 线程的概念

一个程序可能包括多个并发运行的任务。`线程`是指一个任务从头至尾的执行流。线程提供了运行任务的机制。

在 Java 中每个任务都是 Runnable 接口的一个实例，也可以称之为`可运行对象`。线程本质上讲就是便于任务的执行的对象。

#### 2.线程的生命周期

![java 线程生命周期](../_media/images/java-thread.jpg)

- **新建状态**:

  使用 **new** 关键字和 **Thread** 类或其子类建立一个线程对象后，该线程对象就处于新建状态。它保持这个状态直到程序 **start()** 这个线程。

- **就绪状态**:

  当线程对象调用了start()方法之后，该线程就进入就绪状态。就绪状态的线程处于就绪队列中，要等待JVM里线程调度器的调度。

- **运行状态**:

  如果就绪状态的线程获取 CPU 资源，就可以执行 **run()**，此时线程便处于运行状态。处于运行状态的线程最为复杂，它可以变为阻塞状态、就绪状态和死亡状态。

- **阻塞状态**:

  如果一个线程执行了sleep（睡眠）、suspend（挂起）等方法，失去所占用资源之后，该线程就从运行状态进入阻塞状态。在睡眠时间已到或获得设备资源后可以重新进入就绪状态。可以分为三种：

  - 等待阻塞：运行状态中的线程执行 wait() 方法，使线程进入到等待阻塞状态。
  - 同步阻塞：线程在获取 synchronized 同步锁失败(因为同步锁被其他线程占用)。
  - 其他阻塞：通过调用线程的 sleep() 或 join() 发出了 I/O 请求时，线程就会进入到阻塞状态。当sleep() 状态超时，join() 等待线程终止或超时，或者 I/O 处理完毕，线程重新转入就绪状态。

- **死亡状态**:

  一个运行状态的线程完成任务或者其他终止条件发生时，该线程就切换到终止状态。
  
  

**wait() 和 sleep() 的区别**

- wait() 是 Object 的方法，而 sleep() 是 Thread 的静态方法；
- **wait() 会释放锁，sleep() 不会**。


### 二、使用线程

有三种使用线程的方法：

- 实现 Runnable 接口（需要实现 run() 方法）

- 实现 Callable 接口（需要实现 call() 方法，与 Runnable 接口不同的是，call() 方法可以有返回值，可以通过FutureTask封装）

- 继承 Thread 类（同样也是需要实现 run() 方法，因为 Thread 类也实现了 Runable 接口）

实现 Runnable 和 Callable 接口的类只能当做一个可以在线程中运行的任务，不是真正意义上的线程，因此最后还需要通过 Thread 来调用。可以说任务是通过线程驱动从而执行的。

当调用 start() 方法启动一个线程时，虚拟机会将该线程放入就绪队列中等待被调度，当一个线程被调度时会执行该线程的 run() 方法。

```java
public class MyRunnable implements Runnable {
    @Override
    public void run() {
    }
}

public class MyCallable implements Callable<String> {
    @Override
    public String call() {
        return "hello world";
    }
}

public class MyThread extends Thread {
    @Override
    public void run() {
    }
}

public static void main(String[] args) throws ExecutionException, InterruptedException {
    MyCallable mc = new MyCallable();
    FutureTask<String> ft = new FutureTask<>(mc);
    Thread thread = new Thread(ft);
    thread.start();
    System.out.println(ft.get());
}

/**
   实现接口会更好一些，因为：
      Java 不支持多重继承，因此继承了 Thread 类就无法继承其它类，但是可以实现多个接口；
      类可能只要求可执行就行，继承整个 Thread 类开销过大。
*/
```



### 三、线程中断

```java
//中断线程（实例方法）
public void Thread.interrupt();

//判断线程是否被中断（实例方法）
public boolean Thread.isInterrupted();

//判断是否被中断并清除当前中断状态（静态方法）
public static boolean Thread.interrupted();
```

- 当一个线程处于被阻塞状态或者试图执行一个阻塞操作时，使用`Thread.interrupt()`方式中断该线程，注意此时将会抛出一个InterruptedException的异常，从而提前结束该线程，同时中断状态将会被复位(由中断状态改为非中断状态)，但是不能中断 I/O 阻塞和 synchronized 锁阻塞。
- 当中断一个处于运行期且非阻塞的状态的线程时，`Thread.interrupt()` 中断线程不会得到响应，需要我们手动进行中断检测并结束程序（isInterrupted() 和interrupted()方法）。

对线程` interrupt()` 方法的调用先行发生于被中断线程的代码检测到中断事件的发生，可以通过 `interrupted() `方法检测到是否有中断发生。

### 四、线程的互斥与同步

Java 提供了两种锁机制来控制多个线程对共享资源的互斥访问，第一个是 JVM 实现的 synchronized，而另一个是 JDK 实现的 ReentrantLock。



#### synchronized

```java
 // synchronized 同步一个代码块时 只作用与同一个对象上的同步代码块，不同的对象不会进行同步
synchronized (this) {
  ...
}

// synchronized 同步一个方法时，作用与同步一个代码块一样
public synchronized void fun() {
  ...
}

// synchronized 同步一个类时，它作用于整个类，也就是调用同一个类的不同对象上的同步语句，也会进行同步
synchronized (SynchronizedExample.class) {
  ....
}

// synchronized 同步一个静态方法时，作用与同步一个类一样
public synchronized static void fun() {
  ...
}
```



#### synchronized 的底层实现

Java 虚拟机中的同步(Synchronization)基于进入和退出管程(Monitor)对象实现， 无论是显式同步(有明确的 monitorenter 和 monitorexit 指令,即同步代码块)还是隐式同步都是如此。在 Java 语言中，同步用的最多的地方可能是被 synchronized 修饰的同步方法。同步方法 并不是由 monitorenter 和 monitorexit 指令来实现同步的，而是由方法调用指令读取运行时常量池中方法的 ACC_SYNCHRONIZED 标志来隐式实现的。

#### Java 对象头与 Monitor

在JVM中，对象在内存中的布局分为三块区域：对象头、实例数据和对齐填充

![img](https://img-blog.csdn.net/20170603163237166?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvamF2YXplamlhbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

实例变量：存放类的属性数据信息，包括父类的属性信息，如果是数组的实例部分还包括数组的长度，这部分内存按4字节对齐。

对象头：是实现synchronized的锁对象的基础，主要包括 <u>*Mark Word*</u> 和 Class Metadata Address 两部分

| **虚拟机位数** |     **头对象结构**     |                           **说明**                           |
| :------------: | :--------------------: | :----------------------------------------------------------: |
|    32/64bit    |       Mark Word        |      存储对象的hashCode、锁信息或分代年龄或GC标志等信息      |
|    32/64bit    | Class Metadata Address | 类型指针指向对象的类元数据，JVM通过这个指针确定该对象是哪个类的实例。 |

其中Mark Word在默认情况下存储着对象的HashCode、分代年龄、锁标记位等以下是32位JVM的Mark Word默认存储结构

| **锁状态** |    25bit     |   **4bit**   | 1bit是否是偏向锁 | 2bit 锁标志位 |
| :--------: | :----------: | :----------: | :--------------: | :-----------: |
|  无锁状态  | 对象HashCode | 对象分代年龄 |        0         |      01       |

考虑到JVM的空间效率，Mark Word 被设计成为一个非固定的数据结构，以便存储更多有效的数据，它会根据对象本身的状态复用自己的存储空间，如下图：

![img](https://img-blog.csdn.net/20170603172215966?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvamF2YXplamlhbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

我们主要分析一下重量级锁也就是通常说synchronized的对象锁，锁标识位为10，其中指针指向的是monitor对象（也称为管程或监视器锁）的起始地址。<u>**每个对象都存在着一个 monitor 与之关联**</u>，对象与其 monitor 之间的关系有存在多种实现方式，如monitor可以与对象一起创建销毁或当线程试图获取对象锁时自动生成，但当一个 monitor 被某个线程持有后，它便处于锁定状态。在Java虚拟机(HotSpot)中，monitor是由ObjectMonitor实现的，其主要数据结构如下（位于HotSpot虚拟机源码ObjectMonitor.hpp文件，C++实现的）

```c++
ObjectMonitor() {
    _header       = NULL;
    _count        = 0; //记录个数
    _waiters      = 0,
    _recursions   = 0;
    _object       = NULL;
    _owner        = NULL;
    _WaitSet      = NULL; //处于wait状态的线程，会被加入到_WaitSet
    _WaitSetLock  = 0 ;
    _Responsible  = NULL ;
    _succ         = NULL ;
    _cxq          = NULL ;
    FreeNext      = NULL ;
    _EntryList    = NULL ; //处于等待锁block状态的线程，会被加入到该列表
    _SpinFreq     = 0 ;
    _SpinClock    = 0 ;
    OwnerIsThread = 0 ;
  }
```

*ObjectMonitor中有两个队列，  WaitSet 和  EntryList，用来保存ObjectWaiter对象列表( 每个等待锁的线程都会被封装成ObjectWaiter对象)，owner指向持有ObjectMonitor对象的线程，当多个线程同时访问一段同步代码时，首先会进入 EntryList 集合，当线程获取到对象的 monitor 后进入 Owner 区域并把 monitor 中的 owner 变量设置为当前线程同时monitor中的计数器count加1，若线程调用 wait() 方法，将释放当前持有的monitor，owner变量恢复为null，count自减1，同时该线程进入 WaitSe t集合中等待被唤醒。若当前线程执行完毕也将释放monitor(锁)并复位变量的值，以便其他线程进入获取monitor(锁)。*

monitor对象存在于每个Java对象的对象头中(存储的指针的指向)，synchronized锁便是通过这种方式获取锁的，也是为什么Java中任意对象可以作为锁的原因，同时也是<u>**notify/notifyAll/wait等方法存在于顶级对象Object中**</u>的原因（notify/notifyAll和wait方法，在使用这3个方法时，必须处于synchronized代码块或者synchronized方法中，否则就会抛出IllegalMonitorStateException异常，这是因为调用这几个方法前必须拿到当前对象的监视器monitor对象，也就是说notify/notifyAll和wait方法依赖于monitor对象，在前面的分析中，我们知道monitor 存在于对象头的Mark Word 中(存储monitor引用指针)，而synchronized关键字可以获取 monitor ，这也就是为什么notify/notifyAll和wait方法必须在synchronized代码块或者synchronized方法调用的原因）

```java
synchronized (obj) {
       obj.wait();
       obj.notify();
       obj.notifyAll();         
 }
```





#### 参考资料：

- [深入理解Java并发之synchronized实现原理](https://blog.csdn.net/javazejian/article/details/72828483)
- [CS-Notes  java 并发](https://cyc2018.github.io/CS-Notes/#/notes/Java%20%E5%B9%B6%E5%8F%91?id=sleep)