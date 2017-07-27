----
title: Java并发编程（2）：Java并发编程基础
category: Java并发编程
data: 2017-07-27 11:20:21
tags:
	- Java
	- 并发编程
----

 
## 1. 线程简介

现代操作系统调度的最小单元是线程，也叫轻量级进程，在一个进程中可以创建多个线程，并且线程之间可以访问共享的内存变量。处理器在这些线程上高速切换，因此我们感觉这些线程在同时执行。

### 1.1 为什么要使用多线程？

1. 更多的处理核心
2. 更快的响应时间
3. 更好的编程模型

<!--more-->

### 1.2 线程的状态：

* NEW : 初始状态，线程被构建，但是还没有调用start()方法
* RUNNABLE : 运行状态，Java线程将操作系统中的就绪和运行两种状态统称为“运行中”
* BLOCKED : 阻塞状态，表示线程阻塞于锁
* WAITING : 等待状态，表示线程进如等待状态，需要等待其他线程作出一些特定动作（通知或中断）
* TIME_WAITING : 超时等待状态，该状态不同于WAITING状态，它可以在指定的时间自行返回
* TERMINATED :  终止状态，表示当前线程已经执行完毕

### 1.3 Java线程状态变迁图

![Java线程状态变迁](http://static.oschina.net/uploads/space/2016/1020/212748_11NT_1789589.jpg)

### 1.4 Daemon线程是什么？

Daemon线程是一种支持性线程，因为它主要被用作程序中后台调度以及支持性工作。也就是说，当一个Java虚拟机中不存在非Daemon线程的时候，Java虚拟机将会退出。可以通过调用Thread.setDaemon(true)将线程设置为Daemon线程。

注意：在构建Daemon线程时，不能依靠finally块中的内容来执行关闭或清理资源的逻辑。（不会执行）

## 2. 启动和终止线程

### 2.1 构造线程

在运行线程之前首先要构造一个线程对象，线程对象在构造的时候需要提供线程所需要的属性。下面是Thread类的初始化方法：

```Java
private void init(ThreadGroup g, Runnable target, String name,
                      long stackSize, AccessControlContext acc,
                      boolean inheritThreadLocals) {
        if (name == null) {
            throw new NullPointerException("name cannot be null");
        }
        this.name = name;
        Thread parent = currentThread();
        SecurityManager security = System.getSecurityManager();
        if (g == null) {
            if (security != null) {
                g = security.getThreadGroup();
            }
            if (g == null) {
                g = parent.getThreadGroup();
            }
        }
        g.checkAccess();
        /*
         * 我们是否有权限?
         */
        if (security != null) {
            if (isCCLOverridden(getClass())) {
                security.checkPermission(SUBCLASS_IMPLEMENTATION_PERMISSION);
            }
        }
        g.addUnstarted();
        this.group = g;
        this.daemon = parent.isDaemon();
        this.priority = parent.getPriority();
        if (security == null || isCCLOverridden(parent.getClass()))
            this.contextClassLoader = parent.getContextClassLoader();
        else
            this.contextClassLoader = parent.contextClassLoader;
        this.inheritedAccessControlContext =
                acc != null ? acc : AccessController.getContext();
        this.target = target;
        setPriority(priority);
        if (inheritThreadLocals && parent.inheritableThreadLocals != null)
            this.inheritableThreadLocals =
                ThreadLocal.createInheritedMap(parent.inheritableThreadLocals);
        /* 存放指定的栈大小 */
        this.stackSize = stackSize;
        /* 设置线程Id */
        tid = nextThreadID();
    }
```
一个新构造的线程对象是由其parent线程来进行空间分配的，而child线程继承了parent是否为Daemon、优先级和加载资源的contexClassLoader以及可继承的ThreadLocal，同时还会分配一个唯一的Id来标识这个child线程。

### 2.2 启动线程

在线程对象初始化完成之后，调用start()方法就可以启动这个线程。
注意：线程启动之前，最好为这个线程设置线程名称。

### 2.3 中断

中断可以理解为线程的一个标识位属性，它表示一个运行中的线程是否被其他线程进行了中断操作。中断好比其他线程打了个招呼，其他线程通过调用该线程的interrupt()方法对其进行中断操作。

线程通过检查自身是否被中断来进行响应，线程通过方法isInterrupted()来进行判断是否被中断，也可以调用静态方法Thread.interrupted()来对当前线程的中断标识进行复位。如果该线程已经处于终止状态，即使该线程被中断过，在调用该线程的isInterrupted()时依旧会返回false。

### 2.4 过期的 suspend()、resume() 和 stop() 方法

* suspend(): 暂停线程 
* resume(): 恢复线程
* stop(): 停止线程

以上方法均为过期的方法，也是不建议使用的。而具体的暂停和恢复操作可以用后面的等待／通知机制来替代。

### 2.5 安全的终止线程

前面提到中断状态是线程的一个标识位，而中断操作是一种简便的线程见交互方式，而这种交互方式最适合用来取消或停止任务。除了中断操作以外，还可以利用一个boolean变量来控制是否需要停止任务并终止线程。

```Java
public class Shutdown {
    public static void main(String[] args) throws Exception {
        Runner one = new Runner();
        Thread countThread = new Thread(one, "CountThread");
        countThread.start();
        // 睡眠1秒，main线程对CountThread进行中断，使CountThread能够感知中断而结束
        TimeUnit.SECONDS.sleep(1);
        countThread.interrupt();
        Runner two = new Runner();
        countThread = new Thread(two, "CountThread");
        countThread.start();
        // 睡眠1秒，main线程对Runner two进行取消，使CountThread能够感知on为false而结束
        TimeUnit.SECONDS.sleep(1);
        two.cancel();
    }

    private static class Runner implements Runnable {
        private long             i;

        private volatile boolean on = true;

        @Override
        public void run() {
            while (on && !Thread.currentThread().isInterrupted()) {
                i++;
            }
            System.out.println("Count i = " + i);
        }

        public void cancel() {
            on = false;
        }
    }
}
```

## 3. 线程间通信

> 线程开始运行后，拥有自己的栈空间，就会如同脚本一样，按照既定的代码一步步的执行，直到终止。但是，如果仅仅是孤立地运行，那么没什么价值，但是如果多个线程能够互相配合完成工作，这则将会带来巨大的价值。

### 3.1 volatile 和 synchronized 关键字

Java支持多个线程同时访问一个对象或着对象的成员变量。

关键字 volatile 可以用来修饰字段（成员变量），就是告知程序任何对该变量的访问均需要从内存共享中获取，而对它的改变必须同步刷新会共享内存，它能保证所有线程对变量访问的**可见性**。

关键字 synchronized 可以修饰方法或者以同步块的形式来进行使用，它主要确保多个线程在同一个时刻，只能有一个线程处于方法或者同步块中，它保证了线程对变量访问的**可见性**和**排他性**。 

### 3.2 等待／通知机制

一个线程修改了一个对象的值，而另一个线程感知了变化，然后进行响应的操作，整个过程开始于一个线程，而最终执行又是一个另一个线程。就相当于前者是“**生产者**”，后者是“**消费者**”。Java通过内置的等待／通知机制来实现以上过程。等待／通知的相关的方法是任意的Java对象都具备的，因为这些方法被定义在所有对象的超类 java.lang.Object 上，方法和描述如下：

<style>
table th:first-of-type {
    width: 80px;
}
</style>

| 方法名称 | 描述 | 
| -------- | ----------- |
| notify() | 通知一个在对象上等待的线程，由WAITING状态变为BLOCKING状态，从等待队列移动到同步队列，等待CPU调度获取该对象的锁，当该线程获取到了对象的锁后，该线程从wait()方法返回 |
| notifyAll() | 通知所有等待在该对象上的线程，由WAITING状态变为BLOCKING状态，等待CPU调度获取该对象的锁 |
| wait() | 调用该方法的线程进入WAITING状态，并将当前线程放置到对象的等待队列，只有等待另外线程的通知或被中断才会返回，需要注意，调用wait()方法后，会释放对象的锁 |
| wait(long) | 超时等待一段时间，这里的参数时间是毫秒，也就是等待长达n毫秒，如果没有通知就超时返回 |
| wait(long，int) | 对于超时时间更细力度的控制，可以达到纳秒 |


### 3.3 等待／通知经典范式

等待方遵循以下原则：

1. 获取对象的锁
2. 如果条件不满足，那么调用对象的wait()方法，被通知后仍要检查条件。
3. 条件满足则执行对应的逻辑

对应的伪代码如下：

```
synchronized(对象) {
	while(条件不满足) {
		对象.wait();
	}
	对象的处理逻辑
}
```

通知方遵循如下原则：

1. 获得对象的锁
2. 改变条件
3. 通知所有等待在对象上的线程

对应的伪代码如下：

```
synchronized(对象) {
	改变条件
	对象.notifyAll();
}
```





