#### AQS:队列同步器，它是用来构建锁和其他同步组件的基础框架。是一个抽象类，只有方法没有实现，通过定义模板方法的方式提供了一套实现锁的模板，其最基本的锁实现方式需要子类复写模板：

内部：

/指向同步队列队头，**Node为他的内部类**
private transient volatile Node head;

//指向同步的队尾
private transient volatile Node tail;

//同步状态，0代表锁未被占用，1代表锁已被占用

private volatile int state;

##### state：表示当前锁状态，0表示没用，1表示有人占用了。

##### 同步队列：它是一个双端队列，便于对节点的操作。

注意head为空结点，不存储信息。

当进行lock(),加锁后，判断state的值是否为0，如果是，获得该锁，并将state置为1。如果不是，则将当前

### 线程封装成Node节点，加入同步队列。（**Node的数据结构也可看出，其包含了需要同步的线程本身以及线程的状态，如是否被阻塞，是否等待唤醒，是否已经被取消等**）

每个Node结点内部关联其前继结点prev和后继结点next，方便唤醒下一个线程。



#### 两种模式：SHARED和EXCLUSIVE常量分别代表共享模式和独占模式

所谓共享模式是一个锁允许多条线程同时操作，如信号量 Semaphore 采用的就是基于 AQS 的共享模式实现的

而独占模式则是同一个时间段只能有一个线程对共享资源进行操作，多余的请求线程需要排队等待，如 ReentranLock。

##### 那怎么实现这两者模式呢？

    //AQS中提供的主要模板方法，由子类实现。
    public abstract class AbstractQueuedSynchronizer
        extends AbstractOwnableSynchronizer{
    //独占模式下获取锁的方法
        protected boolean tryAcquire(int arg) {
            throw new UnsupportedOperationException();
        }
    //独占模式下解锁的方法
    protected boolean tryRelease(int arg) {
        throw new UnsupportedOperationException();
    }
    
    //共享模式下获取锁的方法
    protected int tryAcquireShared(int arg) {
        throw new UnsupportedOperationException();
    }
    
    //共享模式下解锁的方法
    protected boolean tryReleaseShared(int arg) {
        throw new UnsupportedOperationException();
    }
---------------------
AQS主要提供模板方法，主要实现，还是靠实现类。看下面，reentrantLock怎么实现独占模式的？

## AQS和ReentrantLock关系

ReentrantLock底层的加锁和解锁就是通过AQS。

当state=0，说明没有线程占用。如果state=1，则封装成Node加入**同步队列**。

当**Condition**调用await()方法后，线程将会加入**等待队列**中（注意这里是等待队列）而当Condition调用signal()方法后，线程将从等待队列转移动同步队列中进行锁竞争。

##### 区别：同步队列是竞争资源时发现被占用了，进入同步队列。等待队列是，还未参加，被人通知满了，需要等待。

ReentrantLock内部存在3个实现类，分别是Sync、NonfairSync、FairSync，其中Sync继承自AQS实现了解锁tryRelease()方法，而NonfairSync(非公平锁)、 FairSync(公平锁)则继承自Sync，实现了获取锁的tryAcquire()方法。
--------------------- 
这样就可以同过类来实现，独占模式。所以模板方法就好在这里，通过实现方法实现，不同的需要实现不同方法，但是底层又是同一套AQS组件。

看下ReentrantLock源码：

```
/默认构造，创建非公平锁NonfairSync
public ReentrantLock() {
    sync = new NonfairSync();
}
//根据传入参数创建锁类型
public ReentrantLock(boolean fair) {
    sync = fair ? new FairSync() : new NonfairSync();
}

//加锁操作
public void lock() {
     sync.lock();
}

/**
 * 非公平锁实现
 首先对同步状态执行CAS操作，尝试把state的状态从0设置为1，如果返回true则代表获取同步状态成功，也就是当前线程获取锁成功，如果失败，则执行acquire（1）再次尝试。
 */
static final class NonfairSync extends Sync {
    //加锁
    final void lock() {
        //执行CAS操作，获取同步状态
        if (compareAndSetState(0, 1))
       //成功则将独占锁线程设置为当前线程  
          setExclusiveOwnerThread(Thread.currentThread());
        else
            //否则再次请求同步状态，
            acquire(1);
    }
}
public final void acquire(int arg) {
    //再次尝试获取同步状态，这里传入参数arg表示要获取同步状态后设置的值(即要设置state的值)，因为要获取锁，而status为0时是释放锁，1则是获取锁
    //tryAcquire(arg)返回true，acquireQueued自然不会执行，这是最理想的，因为毕竟当前线程已获取到锁，如果tryAcquire(arg)返回false，则会执行addWaiter(Node.EXCLUSIVE)进行入队操作
    if (!tryAcquire(arg) &&
        acquireQueued(addWaiter(Node.EXCLUSIVE), arg))
        selfInterrupt();
}

/**
 * 释放锁实现
 */
 //ReentrantLock类的unlock
public void unlock() {
    sync.release(1);
}

//AQS类的release()方法
public final boolean release(int arg) {
    //尝试释放锁
    if (tryRelease(arg)) {

        Node h = head;
        if (h != null && h.waitStatus != 0)
            //唤醒后继结点的线程
            unparkSuccessor(h);
        return true;
    }
    return false;
}

//ReentrantLock类中的内部类Sync实现的tryRelease(int releases) 
protected final boolean tryRelease(int releases) {

      int c = getState() - releases;
      if (Thread.currentThread() != getExclusiveOwnerThread())
          throw new IllegalMonitorStateException();
      boolean free = false;
      //判断状态是否为0，如果是则说明已释放同步状态
      if (c == 0) {
          free = true;
          //设置Owner为null
          setExclusiveOwnerThread(null);
      }
      //设置更新同步状态
      setState(c);
      return free;
  }
```

#### 1.Unsafe类

该类中的方法都是native修饰的，其内部方法操作可以像C的指针一样直接操作内存，所以是非安全的。

unsafe类中的方法都直接调用操作系统底层资源执行相应任务.

方法：

1.挂起和恢复：park()和unpark(),用于挂起线程。

2.内存屏障：相当于某些命令执行顺序(规定流水线指令)的规则，用于防止指令重排。



#### 2.原子操作类(Atomic系列)基于CAS

i++,不是原子性，可分为三步，取值，相加，赋值。

Atomic保证原子性。常用	:

- AtomicBoolean：原子更新布尔类型
- AtomicInteger：原子更新整型
- AtomicLong：原子更新长整型

## 3.CAS

V：内存值 A：预期值 B：成功后的赋值

不必担心多个线程同时操作，他是原子性指令。

#### 问题：ABA问题？

在获取到当前变量V，准备修改为新值U前，另外两个线程已连续修改了两次变量V的值，使得该值又恢复为旧值，这样的话，我们就无法正确判断这个变量是否已被修改过

#### 一般这种情况很少，也一般没什么影响。可以使用某些Atomic类解决。