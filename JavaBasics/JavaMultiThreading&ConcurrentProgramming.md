# JavaMultiThreading&ConcurrentProgramming
>java多线程&并发  

**Java并发知识库（来自Java核心面试知识整理）**
![java](https://github.com/xionghaotian525/JavaLearningRoadMap/blob/main/Res/images/并发编程知识库.png)  

# 目录
- [(1)Java中实现线程的方法](#(1)Java中实现线程的方法)
- [二级标题](#二级标题)
	- [AAA](#aaa)
	- [bbb](#bbb)


## 目录  

- [第一部分](#第一部分)
	-[]()
  	-[]()
- [第二部分](#第二部分)
	-[(1)Java中实现线程的方法](#Java中实现线程的方法)
- [第三部分](#第三部分)

理解Java多线程和并发编程知识是一个系统性的过程，可以按照以下大致的目录或路线来学习：

1. **基础概念和原理**：
   - 理解线程和进程的概念
   - 了解并发和并行的区别
   - 掌握Java中的线程模型
   - 理解线程调度和上下文切换

2. **线程的创建和管理**：
   - 学习创建线程的方式：继承Thread类、实现Runnable接口、使用线程池
   - 理解线程的生命周期：新建、就绪、运行、阻塞、终止
   - 掌握线程的状态转换和线程的中断

3. **共享数据和同步机制**：
   - 学习共享数据的问题和线程安全性
   - 理解临界区、互斥量和信号量的概念
   - 掌握Java中的同步机制：synchronized关键字、Lock接口、ReentrantLock类、Condition接口等

4. **并发工具类**：
   - 学习Java并发包中提供的工具类：Semaphore、CountDownLatch、CyclicBarrier等
   - 掌握线程安全的集合类：ConcurrentHashMap、CopyOnWriteArrayList等

5. **线程间通信**：
   - 理解线程间通信的方式：共享内存、消息传递
   - 学习Java中的线程间通信机制：wait/notify、notifyAll、BlockingQueue等

6. **并发设计模式**：
   - 学习常见的并发设计模式：生产者-消费者模式、读写锁模式、单例模式等

7. **高级并发主题**：
   - 理解并发编程中的原子性、可见性、有序性
   - 学习Java中的原子操作类：AtomicInteger、AtomicReference等
   - 掌握Java内存模型（Java Memory Model，JMM）和 happens-before 原则

8. **性能优化和调试**：
   - 学习并发编程中的常见性能问题和调试技巧
   - 掌握工具和技术：Java Profiler、JConsole、VisualVM等

9. **实践和项目**：
   - 进行实际的多线程和并发编程练习
   - 参与开源项目或个人项目，应用所学知识解决实际问题

以上是一个大致的学习路线，你可以根据自己的实际情况和兴趣深入学习。在学习过程中，建议多阅读相关书籍、文档和经典论文，参与社区讨论和技术分享，通过实践来巩固所学知识。 

### (1)Java中实现线程的方法   
### (2)如何停止一个正在运行的线程  
### (3)notify和notifyAll有什么区别  
### (4)sleep()和wait()有什么区别  
### (5)volatile是什么？可以保证有序性吗？  
### (6)Thread类中的start()和run方法有什么区别？  
(7)为什么wait、notify、notifyAll这些方法不在Thread类里面？  
(8)为什么wait和notify方法要在同步块中调用？  
(9)Java中interrupted和isInterrupted方法的区别  
(10)Java中synchronized 和 ReentrantLock 有什么不同？  
(11)有三个线程T1,T2,T3,如何保证顺序执行？  
(12)SynchronizedMap和ConcurrentHashMap有什么区别？  
(13)什么是线程安全  
(14)Thread类中的yield方法有什么作用？  
(15)Java线程池中submit() 和 execute()方法有什么区别？  
(16)说一说自己对于 synchronized 关键字的了解（重要）  
(17)说说自己是怎么使用 synchronized 关键字？  
(18)什么是线程安全？Vector是一个线程安全类吗？  
(19)volatile关键字的作用？（重要）  
(20)常用的线程池有哪些？  
(21)什么是线程池？简述一下你对线程池的理解  
(22)Java程序是如何执行的  
(23)锁的优化机制了解吗？  
(24)说说进程和线程的区别？  
(25)产生死锁的四个必要条件？  
(26)如何避免死锁？  
(27)线程池核心线程数怎么设置呢？  
(28)Java线程池中队列常用类型有哪些？  
(29)线程安全需要保证几个基本特征？  
(30)说一下线程之间是如何通信的？  
(31)CAS的原理呢？  
(32)CAS有什么缺点吗？  
(33)引用类型有哪些？有什么区别？  
(34)说说ThreadLocal原理？  
(35)线程池原理知道吗？以及核心参数  
(36)线程池的拒绝策略有哪些？  
(37)说说你对JMM内存模型的理解？为什么需要JMM？  
(38)多线程有什么用？为什么要使用多线程  
(39)说说CyclicBarrier和CountDownLatch的区别？  
(40)什么是AQS？  
(41)了解Semaphore吗？  
(42)什么是Callable和Future?   
(43)什么是阻塞队列？阻塞队列的实现原理是什么？如何使用阻塞队列来实现生产者-消费者模型？  
(44)什么是多线程中的上下文切换？  
(45)什么是Daemon线程？它有什么意义？  
(46)乐观锁和悲观锁的理解及如何实现，有哪些实现方式？  
(47)介绍一下automic原子类  

**什么是多线程中的并发切换**  
**1. hello**  
  - ok
  - fine
  
**2. hello**  



> - hello `print()`
> - hello

```java
  System.out.println("hello world()");
```
