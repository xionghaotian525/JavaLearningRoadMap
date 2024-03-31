# JavaMultiThreading&ConcurrentProgramming
>java多线程&并发  

## 目录
[toc]

<!-- - [一、Gemini总结参考](#一gemini总结参考)
  - [基础概念](#基础概念)
  - [线程安全](#线程安全)
  - [线程间通信](#线程间通信)
  - [线程池](#线程池)
  - [并发编程模型](#并发编程模型)
  - [高级主题](#高级主题)
  - [调试与分析](#调试与分析)
- [二、ChatGPT总结参考](#二ChatGPT总结参考)
  - [基础概念](#基础概念)
  - [多线程编程](#多线程编程)
  - [并发编程工具和框架](#并发编程工具和框架)
  - [并发设计模式](#并发设计模式)
  - [并发安全性和性能](#并发安全性和性能)
  - [Java并发框架](#java并发框架)
  - [性能调优和注意事项](#性能调优和注意事项)
- [四、个人笔记](#四个人笔记)
  - []()
  - []()
  - []()
- [三、八股问题整理](#三八股问题整理) 
	- [(1)Java中实现线程的方法](#1java中实现线程的方法) -->

## 一、Gemini总结参考

### 基础概念


**1. 进程与线程:**

* **进程:** 进程是操作系统分配资源的基本单位，拥有独立的内存空间和资源。一个应用程序通常对应一个进程。
* **线程:** 线程是进程内的执行单元，共享进程的内存空间和资源。一个进程可以包含多个线程，并发执行多个任务。

**2. 并发与并行:**

* **并发:** 并发是指多个任务在同一时间段内交替执行。例如，单核CPU上运行多个程序，通过快速切换，宏观上看起来像多个程序同时运行。
* **并行:** 并行是指多个任务在同一时刻同时执行。例如，多核CPU上，每个核心可以同时执行一个任务，真正实现多个任务同时运行。

**3. 线程状态:**

Java线程有以下五种状态：

* **新建 (New):** 线程对象创建后，尚未启动。
* **就绪 (Runnable):** 线程已启动，等待CPU分配时间片执行。
* **运行 (Running):** 线程获得CPU时间片，正在执行。
* **阻塞 (Blocked):** 线程等待某个事件发生，例如获取锁、等待I/O操作等。
* **死亡 (Terminated):** 线程执行完毕或异常终止。

**4. 创建线程:**

Java中创建线程有两种方式：

* **继承Thread类:** 编写一个类继承Thread类，并重写run()方法，将要执行的任务代码放在run()方法中。
* **实现Runnable接口:** 编写一个类实现Runnable接口，并实现run()方法，将要执行的任务代码放在run()方法中。然后创建Thread对象，将Runnable对象作为参数传入Thread构造函数。

**5. 启动线程:**

创建线程对象后，调用start()方法启动线程。start()方法会执行一些初始化操作，并最终调用run()方法执行任务代码。注意，不能直接调用run()方法，否则不会启动新的线程，而是在当前线程中执行run()方法。

**6. 线程生命周期:**

线程的生命周期包括创建、启动、运行、阻塞、死亡五个阶段。线程状态转换如下图所示：

![Java线程状态转换图](/Res/images/thread-life-cycle.png "线程状态转换图")

**7. 常见方法:**

* **start():** 启动线程。
* **run():** 线程执行的任务代码。
* **sleep(long millis):** 线程休眠指定毫秒数。
* **join():** 等待线程执行完毕。
* **interrupt():** 中断线程。

**8. 守护线程:**

守护线程是一种特殊的线程，当所有非守护线程结束时，守护线程会自动结束。例如，垃圾回收线程就是一个守护线程。

**9. 线程优先级:**

每个线程都有一个优先级，优先级高的线程更容易获得CPU时间片。可以通过setPriority()方法设置线程优先级。

**注意:** 以上只是基础概念的概述，每个主题都包含更多细节和复杂性。建议查阅相关书籍和资料，进行更深入的学习。


### 线程安全


**1. 数据竞争:**

当多个线程同时访问共享数据，并且至少有一个线程进行写操作时，就会发生数据竞争。数据竞争会导致数据不一致，程序运行结果不可预测。

**2. 同步机制:**

为了避免数据竞争，需要使用同步机制来控制线程对共享数据的访问。Java中常用的同步机制包括：

* **锁:** 锁是一种互斥机制，同一时刻只有一个线程可以获得锁，其他线程必须等待锁释放才能访问共享数据。Java中常用的锁包括：
* **synchronized关键字:** 可以用于同步方法或代码块。
* **Lock接口:** 提供了更强大的锁功能，例如可重入锁、读写锁等。
* **volatile关键字:** volatile关键字可以保证变量的可见性和部分有序性。当一个变量被声明为volatile时，任何线程对该变量的修改都会立即被其他线程看到。

**3. 线程安全的数据结构:**

Java提供了一些线程安全的数据结构，例如ConcurrentHashMap、CopyOnWriteArrayList等。这些数据结构内部实现了同步机制，可以安全地被多个线程并发访问。

**4. 原子操作:**

原子操作是指不可中断的操作，即使多个线程同时执行原子操作，也不会出现数据竞争问题。Java提供了一些原子操作类，例如AtomicInteger、AtomicLong等。

**5. 线程安全的设计模式:**

一些设计模式可以帮助我们编写线程安全的代码，例如：

* **不变模式:** 创建不可变的对象，避免数据被修改。
* **线程封闭:** 将数据封闭在线程内部，避免数据被其他线程访问。

**6. 常见问题:**

线程安全问题是并发编程中常见的错误，例如：

* **死锁:** 多个线程互相等待对方释放锁，导致所有线程都无法继续执行。
* **活锁:** 多个线程互相谦让，导致所有线程都无法获得锁。
* **饥饿:** 某些线程一直无法获得锁，导致无法执行。

**学习建议:**

* 理解数据竞争的概念，掌握常见的同步机制。
* 使用线程安全的数据结构和原子操作。
* 学习线程安全的设计模式。
* 通过实践和调试，避免常见的线程安全问题。

### 线程间通信

**1. 为什么要进行线程间通信？**

在多线程程序中，线程之间 often 需要相互协调，例如：

* 一个线程需要等待另一个线程完成某个任务才能继续执行。
* 一个线程需要通知另一个线程发生了某个事件。
* 多个线程需要协作完成一个复杂的任务。

**2. 线程间通信的方式:**

Java中常用的线程间通信方式包括：

* **wait()、notify()、notifyAll()方法:** 这些方法用于线程间协调，需要配合synchronized关键字使用。
* **Condition接口:** Condition接口提供了更强大的线程间通信机制，可以创建多个条件变量，实现更精细的线程控制。
* **BlockingQueue:** BlockingQueue是一种阻塞队列，可以用于线程间数据传递。生产者线程可以将数据放入队列，消费者线程可以从队列中取出数据。

**3. wait()、notify()、notifyAll()方法:**

* **wait():** 调用wait()方法的线程会释放锁，并进入等待状态。
* **notify():** 调用notify()方法会唤醒一个等待该锁的线程。
* **notifyAll():** 调用notifyAll()方法会唤醒所有等待该锁的线程。

**4. Condition接口:**

Condition接口提供了类似wait()、notify()、notifyAll()方法的功能，但更加灵活。可以使用Condition对象创建多个条件变量，实现更精细的线程控制。

**5. BlockingQueue:**

BlockingQueue是一种阻塞队列，常用的BlockingQueue实现类包括：

* **ArrayBlockingQueue:** 基于数组的阻塞队列。
* **LinkedBlockingQueue:** 基于链表的阻塞队列。
* **PriorityBlockingQueue:** 优先级阻塞队列。

**6. 示例:**

以下是一个使用wait()、notify()方法进行线程间通信的示例：

```java
public class WaitNotifyExample {

   private Object lock = new Object();

   public void producer() throws InterruptedException {
      synchronized (lock) {
         // 生产数据
         lock.notify(); // 通知消费者线程
      }
   }

public void consumer() throws InterruptedException {
      synchronized (lock) {
         lock.wait(); // 等待生产者线程通知
         // 消费数据
      }
   }
}
```

**学习建议:**

* 理解线程间通信的必要性。
* 掌握wait()、notify()、notifyAll()方法和Condition接口的使用。
* 学习BlockingQueue的使用。
* 通过实践和调试，解决线程间通信问题。

### 线程池

**1. 什么是线程池？**

线程池是一种管理线程的机制，它可以预先创建一些线程，并将任务提交到线程池中执行。线程池可以提高程序的性能和效率，并降低资源消耗。

**2. 使用线程池的好处:**

* **提高效率:** 线程池可以避免频繁创建和销毁线程的开销。
* **降低资源消耗:** 线程池可以控制线程数量，避免创建过多线程导致资源耗尽。
* **方便管理:** 线程池可以统一管理线程，简化线程管理工作。

**3. ExecutorService接口:**

ExecutorService接口是Java线程池的核心接口，它提供了管理线程池的方法，例如提交任务、关闭线程池等。

**4. ThreadPoolExecutor类:**

ThreadPoolExecutor类是ExecutorService接口的常用实现类，它提供了丰富的配置参数，可以创建各种类型的线程池。

**5. 线程池的配置参数:**

ThreadPoolExecutor类常用的配置参数包括：

* **corePoolSize:** 核心线程数，线程池中始终保持的最小线程数。
* **maximumPoolSize:** 最大线程数，线程池中允许的最大线程数。
* **keepAliveTime:** 线程空闲时间，超过该时间后空闲线程会被销毁。
* **workQueue:** 任务队列，用于存放待执行的任务。

**6. 线程池的拒绝策略:**

当线程池的任务队列已满，并且线程数已达到最大线程数时，线程池会拒绝新的任务。常用的拒绝策略包括：

* **AbortPolicy:** 抛出RejectedExecutionException异常。
* **DiscardPolicy:** 丢弃新任务。
* **DiscardOldestPolicy:** 丢弃队列中最旧的任务。
* **CallerRunsPolicy:** 在调用者线程中执行新任务。

**7. 示例:**

以下是一个使用ThreadPoolExecutor类创建线程池的示例：

```java
ExecutorService executor = new ThreadPoolExecutor(
   5, // 核心线程数
   10, // 最大线程数
   60, // 线程空闲时间
   TimeUnit.SECONDS, // 时间单位
   new LinkedBlockingQueue<>() // 任务队列
);

// 提交任务到线程池
executor.execute(() -> {
// 任务代码
});

// 关闭线程池
executor.shutdown();
```

**学习建议:**

* 理解线程池的概念和工作原理。
* 掌握ExecutorService接口和ThreadPoolExecutor类的使用。
* 学习配置线程池参数和拒绝策略。
* 通过实践和调试，优化线程池的使用。

### 并发编程模型

**1. 什么是并发编程模型？**

并发编程模型是一种用于设计和实现并发程序的抽象模型。不同的并发编程模型有不同的特点和适用场景。

**2. 常用的并发编程模型:**

Java中常用的并发编程模型包括：

* **Fork/Join框架:** Fork/Join框架是一种用于并行处理任务的框架。它将一个大任务分解成多个小任务，并使用多个线程并行执行小任务，最后将小任务的结果合并成大任务的结果。
* **Actor模型:** Actor模型是一种基于消息传递的并发模型。每个Actor都是一个独立的实体，可以接收和处理消息。Actor之间通过消息传递进行通信。

**3. Fork/Join框架:**

Fork/Join框架的核心思想是分治法。它将一个大任务分解成多个小任务，并使用多个线程并行执行小任务，最后将小任务的结果合并成大任务的结果。Fork/Join框架使用ForkJoinPool类来管理线程池，使用ForkJoinTask类来表示任务。

**4. Actor模型:**

Actor模型是一种基于消息传递的并发模型。每个Actor都是一个独立的实体，可以接收和处理消息。Actor之间通过消息传递进行通信。Actor模型的优点是易于理解和实现，并且可以避免数据竞争问题。

### 高级主题

**1. CAS操作:**

CAS操作（Compare And Swap）是一种用于实现无锁并发的技术。CAS操作包含三个参数：内存地址、预期值和新值。CAS操作会比较内存地址中的值和预期值，如果相等，则将内存地址中的值更新为新值，并返回true；否则，不更新内存地址中的值，并返回false。

CAS操作可以用于实现无锁的数据结构，例如AtomicInteger类。AtomicInteger类使用CAS操作来实现原子性的incrementAndGet()方法。

**2. AQS框架:**

AQS框架（AbstractQueuedSynchronizer）是一个用于构建锁和同步器的框架。AQS框架提供了一个基于FIFO队列的同步机制，可以用于实现各种锁和同步器，例如ReentrantLock、Semaphore等。

AQS框架的核心思想是使用一个volatile int类型的变量来表示同步状态。线程可以通过CAS操作来获取或释放同步状态。如果线程获取同步状态失败，则会进入FIFO队列等待。

**3. Java内存模型:**

Java内存模型（Java Memory Model，JMM）定义了Java程序中线程如何访问内存。JMM是一个抽象模型，它屏蔽了不同硬件平台和操作系统的差异，为Java程序员提供了一个一致的内存访问模型。

JMM的核心思想是“ happens-before ”关系。如果一个操作happens-before另一个操作，则第一个操作的结果对第二个操作可见。JMM定义了一系列happens-before规则，例如：

* **程序顺序规则:** 同一个线程中的操作，按照程序顺序执行。
* **volatile变量规则:** 对volatile变量的写操作happens-before对该变量的读操作。
* **锁规则:** 解锁操作happens-before对同一个锁的加锁操作。

理解Java内存模型对编写并发程序至关重要。

**学习建议:**

* 理解CAS操作、AQS框架和Java内存模型的概念和工作原理。
* 学习如何使用CAS操作和AQS框架构建无锁数据结构和同步器。
* 学习Java内存模型的happens-before规则，避免并发程序中的数据竞争问题。

### 调试与分析

**1. 调试工具:**

Java提供了一些工具可以帮助我们调试和分析多线程程序，例如：

* **jstack:** jstack命令可以打印Java进程的线程堆栈信息，可以用于分析线程状态、死锁等问题。
* **jconsole:** jconsole是一个图形化的监控工具，可以监控Java进程的CPU使用率、内存使用率、线程状态等信息。
* **VisualVM:** VisualVM是一个功能强大的监控和分析工具，可以监控Java进程的各种性能指标，并进行性能分析。

**2. 并发Bug:**

并发程序中常见的Bug包括：

* **死锁:** 多个线程互相等待对方释放锁，导致所有线程都无法继续执行。
* **活锁:** 多个线程互相谦让，导致所有线程都无法获得锁。
* **饥饿:** 某些线程一直无法获得锁，导致无法执行。
* **数据竞争:** 多个线程同时访问共享数据，导致数据不一致。

**3. 调试技巧:**

调试并发程序比调试单线程程序更困难，因为并发程序的行为难以预测。以下是一些调试并发程序的技巧：

* **重现问题:** 首先要尽可能重现问题，才能进行调试。
* **使用调试工具:** 使用jstack、jconsole等工具可以帮助我们分析线程状态、死锁等问题。
* **添加日志:** 在代码中添加日志可以帮助我们跟踪线程的执行过程。
* **单步调试:** 使用IDE的单步调试功能可以帮助我们逐步分析代码的执行过程。

**4. 学习建议:**

* 学习使用jstack、jconsole等调试工具。
* 了解常见的并发Bug，并学习如何避免这些Bug。
* 通过实践和调试，积累调试并发程序的经验。

## 二、ChatGPT总结参考

### 基础概念
1. **线程（Thread）**：
   - 线程是程序执行的最小单位，Java 中可以使用 `java.lang.Thread` 类或者实现 `java.lang.Runnable` 接口来创建和管理线程。

2. **进程（Process）**：
   - 进程是正在运行的程序的实例，每个 Java 程序都至少有一个进程。每个进程都有它自己的内存空间。

3. **并发（Concurrency）**：
   - 并发是指多个任务在同一时间段内同时进行。

4. **并行（Parallelism）**：
   - 并行是指多个任务在同一时刻执行，通常需要多核处理器或多台计算机。

### 多线程编程
1. **线程的创建**：
   - 继承 `Thread` 类并重写 `run()` 方法，或者实现 `Runnable` 接口并传递给 `Thread` 对象。
   - 使用 Java 8 中的 Lambda 表达式简化 `Runnable` 接口的实现。

2. **线程的生命周期**：
   - 新建（New）
   - 就绪（Runnable）
   - 运行（Running）
   - 阻塞（Blocked）
   - 终止（Terminated）

3. **线程的调度**：
   - 线程调度器负责决定哪个线程在某一时刻运行。
   - 可以通过设置线程的优先级来影响调度器的决策。

4. **线程的同步**：
   - 同步是为了避免多个线程访问共享资源时发生竞态条件（Race Condition）。
   - 使用 `synchronized` 关键字或者 `ReentrantLock` 类来创建临界区。
   - 使用 `volatile` 关键字保证可见性。

5. **线程的通信**：
   - 使用 `wait()`、`notify()` 和 `notifyAll()` 方法实现线程间的通信。
   - 也可以使用 `Lock` 和 `Condition` 接口提供的方法来实现类似的功能。

6. **线程池**：
   - 线程池管理着多个工作线程，可以有效地重用线程并控制并发任务的数量。
   - Java 提供了 `java.util.concurrent.Executor` 和 `ThreadPoolExecutor` 类来实现线程池。

### 并发编程工具和框架
1. **java.util.concurrent 包**：
   - 提供了许多并发编程的工具和数据结构，如 `ConcurrentHashMap`、`CountDownLatch`、`Semaphore` 等。

2. **并发集合**：
   - 提供了线程安全的集合类，如 `ConcurrentHashMap`、`ConcurrentLinkedQueue` 等。

3. **原子操作类**：
   - 提供了一系列原子操作，如 `AtomicInteger`、`AtomicLong` 等，保证了线程安全性。

4. **并行流（Parallel Streams）**：
   - Java 8 引入的并行流 API，可以轻松地将串行流转换为并行流，实现并行处理数据流。

### 并发设计模式
1. **生产者-消费者模式**：
   - 通过一个或多个生产者将任务放入队列，由一个或多个消费者从队列中取出任务并处理。

2. **读写锁模式**：
   - 允许多个线程同时读取共享资源，但只允许一个线程写入共享资源。

3. **Future 模式**：
   - 提供了一种异步处理任务的方式，允许提交一个任务并在后续获取任务的执行结果。

4. **Actor 模式**：
   - 使用轻量级的并发单元（称为 Actor）来处理消息。

### 并发安全性和性能
1. **死锁（Deadlock）**：
   - 发生在两个或多个线程互相等待对方持有的资源时，导致它们都无法继续执行的情况。

2. **竞态条件（Race Condition）**：
   - 多个线程同时访问共享资源，导致最终的结果取决于执行的时序。

3. **性能调优**：
   - 避免过多的线程竞争。
   - 减少锁的持有时间。
   - 使用高效的数据结构和算法。

### Java并发框架
1. **Fork/Join 框架**：
   - 用于将一个大任务拆分成多个小任务并行执行的框架。

2. **并发编程工具**：
   - `java.util.concurrent` 包中提供了许多实用的并发编程工具，如 `Executor`、`Semaphore`、`CountDownLatch` 等。

3. **并发集合**：
   - 提供了线程安全的集合类，如 `ConcurrentHashMap`、`ConcurrentLinkedQueue` 等。

4. **CompletableFuture**：
   - Java 8 引入的用于异步编程的类，提供了更方便的方式来处理异步任务和其结果。

### 性能调优和注意事项
1. **避免过多的线程创建**：
   - 太多的线程会增加线程切换的开销，降低性能。

2. **减少锁的持有时间**：
   - 尽可能缩小同步代码块的范围，减少锁的竞争。

3. **使用线程池**：
   - 重用线程可以减少线程创建和销毁的开销。

4. **使用无锁数据结构**：
   - 无锁数据结构可以减少线程间的竞争，提高并发性能。例如，使用 `java.util.concurrent.atomic` 包中的原子类。

5. **避免线程阻塞**：
   - 长时间的阻塞可能会导致资源浪费和性能下降。可以使用非阻塞算法或者异步编程模型来避免线程阻塞。

6. **注意线程安全性**：
   - 确保共享资源的线程安全性，避免出现数据竞争和不一致的状态。

7. **避免饥饿和死锁**：
   - 使用适当的同步策略和算法来避免线程饥饿和死锁。

8. **使用合适的并发模型**：
   - 根据应用的需求选择合适的并发模型，例如使用 Actor 模型、消息传递模型等。

9.  **进行性能测试和调优**：  
    - 使用性能测试工具和技术对并发程序进行测试和调优，找出性能瓶颈并进行优化。
10. **理解并发编程的挑战**：
    - 并发编程具有一定的复杂性和挑战性，需要深入理解并发原理和各种并发机制，以及它们在实际应用中的使用。

## 三、个人笔记

### 1.线程基础
1. **线程的基本概念**
   - 线程是程序执行流的最小单元，是操作系统能够进行调度的基本单位。一个进程可以包含一个或多个线程。
   - 特点：并发性、独立性、共享性
2. **线程、进程和程序**
   - 程序是一组指令的集合，它是静态的，存储在磁盘上。程序本身不能执行，需要被加载到内存中才能运行。
   - 进程是程序的一次执行过程，它是动态的，拥有自己的内存空间、资源和状态。一个程序可以对应多个进程。
   - 线程是进程中的执行单元，它共享进程的内存空间和资源，但拥有自己的状态。一个进程可以包含多个线程。

| 特征 | 程序 | 进程 | 线程 |
|---|---|---|---|
| 定义 | 指令集合 | 程序的一次执行过程 | 进程中的执行单元 |
| 动态性 | 静态 | 动态 | 动态 |
| 资源 | 无 | 独立的内存空间和资源 | 共享进程的内存空间和资源 |
| 状态 | 无 | 独立的状态 | 独立的状态 |
| 数量 | 一个程序可以对应多个进程 | 一个进程可以包含多个线程 |
3. **java线程创建（实现）方式**
   - 继承Thread类

   ```java
   public class MyThread extends Thread { 
      public void run() { 
         System.out.println("MyThread.run()"); 
      } 
   } 
   MyThread myThread1 = new MyThread(); 
   myThread1.start();
   ```
   - 实现Runnable接口

   ```java
   //如果自己的类已经 extends 另一个类，就无法直接 extends Thread，此时，可以实现一个Runnable 接口
   public class MyThread extends OtherClass implements Runnable { 
      public void run() { 
         System.out.println("MyThread.run()"); 
      } 
   } 
   //启动 MyThread，需要首先实例化一个 Thread，并传入自己的 MyThread 实例：
   MyThread myThread = new MyThread(); 
   Thread thread = new Thread(myThread); 
   thread.start(); 
   //事实上，当传入一个 Runnable target 参数给 Thread 后，Thread 的 run()方法就会调用
   target.run()
   public void run() { 
      if (target != null) { 
         target.run(); 
      } 
   }
   ```
   - 实现Callable接口

   有返回值的任务必须实现 Callable 接口，类似的，无返回值的任务必须 Runnable 接口。执行Callable 任务后，可以获取一个 Future 的对象，在该对象上调用 get 就可以获取到 Callable 任务返回的 Object 了，再结合线程池接口 ExecutorService 就可以实现传说中**有返回结果的多线程**了。
   ```java
   //创建一个线程池
   ExecutorService pool = Executors.newFixedThreadPool(taskSize);
   // 创建多个有返回值的任务
   List<Future> list = new ArrayList<Future>(); 
   for (int i = 0; i < taskSize; i++) { 
      Callable c = new MyCallable(i + " "); 
      // 执行任务并获取 Future 对象
      Future f = pool.submit(c); 
      list.add(f); 
   } 
   // 关闭线程池
   pool.shutdown(); 
   // 获取所有并发任务的运行结果
   for (Future f : list) { 
      // 从 Future 对象上获取任务的返回值，并输出到控制台
      System.out.println("res：" + f.get().toString()); 
   }
   ```
   - 线程池方式创建

   线程和数据库连接这些资源都是非常宝贵的资源。那么每次需要的时候创建，不需要的时候销毁，是非常浪费资源的。那么我们就可以使用缓存的策略，也就是使用线程池。
   ```java
   // 创建线程池
   ExecutorService threadPool = Executors.newFixedThreadPool(10);
   while(true) {
   threadPool.execute(new Runnable() { // 提交多个线程任务，并执行
      @Override
      public void run() {
            System.out.println(Thread.currentThread().getName() + " is running ..");
            try {
               Thread.sleep(3000);
            } catch (InterruptedException e) {
               e.printStackTrace();
            }
         }
      });
   }
   
   ```
4. **线程状态**
   新建（New）、就绪（Runnable）、运行（Running）、阻塞（Blocked）、等待（Waiting）、超时等待（Timed_Waiting）和终止（Terminated）。
   ![线程状态](/Res/images/life-cycle-of-a-thread.png "线程状态")
   ![线程状态转变](/Res/images/thread-life-cycle.png "状态转变")
5. **线程控制**
![](/Res/images/线程状态变迁（图源《java并发编程艺术》）.png)   
   - 线程调度
     - **yield**
       - yield() 是一个静态方法，用于暗示当前执行的线程愿意放弃其当前使用的处理器资源。
       - 当一个线程调用 yield() 方法时，它给调度器（线程调度器）一个暗示，表明当前线程愿意让出对处理器的控制，以便调度器可以运行其他同等优先级的线程。
       - 实际上，yield() 并不保证使得其他线程一定能够获得执行权，也不影响线程的状态。当前线程仍然处于可运行状态（RUNNABLE），只是给了调度器一个可以执行其他线程的机会。
     - **join**
       - join() 方法允许一个线程等待另一个线程完成之后再继续执行。
       - 当在一个线程A中调用另一个线程B的 join() 方法时，线程A将进入等待状态直到线程B完成，或者达到了join() 指定的等待时间（如果设置了超时时间的话）。
       - 这通常用于在开始运行依赖于其他线程执行结果的代码之前，确保这些线程已经运行完成。
     - **priority**
       - 在Java中，每个线程都有一个优先级，优先级由简单的整数表示，范围从 `Thread.MIN_PRIORITY` （值为1）到 `Thread.MAX_PRIORITY` （值为10）。默认优先级是 `Thread.NORM_PRIORITY`（值为5）。
       - 通过 `setPriority()` 方法可以改变线程的优先级。
       - 调度器将尝试优先执行具有较高优先级的线程。然而，线程优先级的处理是依赖于操作系统的调度策略，并且并不是一个严格的保证。在某些操作系统上，优先级可能几乎无影响。
   - 等待/通知
      wait(), notify()和notifyAll()是**Object类**的一部分，它们用于线程间的协调和通信。这些方法为等待和通知模式提供了一种机制，让一个线程暂停执行（等待状态）直到另外的线程通知它一些条件状态的变化。这些方法通常与同步代码块一起使用，来确保线程间的安全交互和避免竞态条件。
     - **wait()**
       - 当一个线程调用共享对象的wait()方法时，它会挂起自己的执行，并释放该对象上的锁，从而使其他线程可以进入同步代码块并获取该对象的锁。
       - 调用wait()的线程将进入该对象的等待池，直到另一个线程在同一个对象上调用notify()或notifyAll()。
       - 调用wait()时应始终在一个while循环中检查条件，以防止虚假唤醒和确保条件确实满足。
     - **notify()**
       - 当一个线程调用共享对象的notify()方法时，它会随机选择在该对象的等待池中等待的一个线程，并通知它可以继续执行。
       - 这个待唤醒的线程将从等待池移动到同步队列，并等待获取对象上的锁。
       - notify()并不立即释放锁；调用notify()的线程在退出同步代码块后，才会释放锁。
     - **notifyAll()**
       - 这个方法会唤醒在该对象等待池中等待的所有线程，而不仅仅是一个。
       - 所有被唤醒的线程将被移动到同步队列，并开始竞争对象上的锁。只有一个线程会赢得竞争并能够继续执行；其他线程将继续在同步队列中等待获得锁。
       - 类似于notify()，调用notifyAll()的线程在退出同步代码块后，才会释放锁。
   - 中断机制
     - interrupt方法
     - InterruptedException
### 2.线程池
   - **线程池原理：**

线程池的工作原理是预先创建一些线程放入一个池子（也就是队列）中，这些线程都是处于休眠状态，也就是空闲状态。当有任务提交时，从池子中取出一个线程去执行这个任务，执行完该任务的线程并不会被销毁，而是再次返回到池子中等待下一次使用。 
   - **线程池优点：**
      - 降低资源消耗。通过重复利⽤已创建的线程降低线程创建和销毁造成的消耗。
      - 提⾼响应速度。当任务到达时，任务可以不需要的等到线程创建就能⽴即执⾏。
      - 提⾼线程的可管理性。线程是稀缺资源，如果⽆限制的创建，不仅会消耗系统资源，还会降低系统的稳定性，使⽤线程池可以进⾏统⼀的分配，调优和监控。
   - **线程池的组成：**
     - **线程池管理器（ThreadPoolExecutor）**：用于创建并管理线程池，包含创建线程池，销毁线程池，添加新任务。
     -  **工作线程（Worker）**：线程池中的线程。在没有任务时，它们会处于等待状态，可以循环的执行任务。
     - **任务接口（Runnable/Callable）**：每个任务必须实现的接口，以定义任务的入口，以供工作线程调用。
     - **任务队列（BlockingQueue）**：用于存放待处理的任务。提供了缓存机制。
     - **线程工厂（ThreadFactory）**：用来创建线程。
   - Executor 框架：提供线程池管理的高级接口和类（如 `ExecutorService`，`ScheduledExecutorService`，`AbstractExecutorService`）。
   - `FixedThreadPool`:

   **创建一个可重用固定线程数的线程池，以共享的无界队列方式来运行这些线程**。在任意点，在大多数 nThreads 线程会处于处理任务的活动状态。如果在所有线程处于活动状态时提交附加任务，则在有可用线程之前，附加任务将在队列中等待。如果在关闭前的执行期间由于失败而导致任何线程终止，那么一个新线程将代替它执行后续的任务（如果需要）。在某个线程被显式地关闭之前，池中的线程将一直存在。
   - `CachedThreadPool`:

   创建一个可根据需要创建新线程的线程池，但是在以前构造的线程可用时将重用它们。对于执行很多短期异步任务的程序而言，这些线程池通常可提高程序性能。**调用 execute 将重用以前构造的线程（如果线程可用）。如果现有线程没有可用的，则创建一个新线程并添加到池中。终止并从缓存中移除那些已有 60 秒钟未被使用的线程**。因此，长时间保持空闲的线程池不会使用任何资源。
   - `ScheduledThreadPool`: 

   创建一个线程池，它可安排在给定延迟后运行命令或者定期地执行。
   ```java
   ScheduledExecutorService scheduledThreadPool= Executors.newScheduledThreadPool(3); 
   scheduledThreadPool.schedule(newRunnable(){ 
      @Override 
      public void run() {
         System.out.println("延迟三秒");
      }
   }, 3, TimeUnit.SECONDS);
   scheduledThreadPool.scheduleAtFixedRate(newRunnable(){ 
      @Override 
      public void run() {
         System.out.println("延迟 1 秒后每三秒执行一次");
      }
   },1,3,TimeUnit.SECONDS);
   ```
   - `SingleThreadExecutor`:

Executors.newSingleThreadExecutor()返回一个线程池（这个线程池只有一个线程）,这个线程池可以在线程死后（或发生异常时）重新启动一个线程来替代原来的线程继续执行下去！
   - ThreadPoolExecutor：线程池管理器，可高度定制的线程池实现。
   - **ThreadPoolExecutor核心参数：**
     - **corePoolSize：** 核心线程数，线程池创建时候，线程的数量，当线程池中的线程数目达到corePoolSize后，就会把到达的任务放到队列中。
     - **maximumPoolSize：** 线程池最大线程数，这个参数也就是线程池能够容纳同时执行的最大线程数，超出的线程会被队列缓存。
     - **workQueue：** 任务队列，被提交但尚未被执行的任务。
     - **keepAliveTime：** 线程空闲时间，当线程空闲时间达到keepAliveTime后，线程会被销毁，直到只剩下corePoolSize个线程为止。
     - **unit：** 时间单位，keepAliveTime的单位。
     - **threadFactory：** 线程工厂，用于创建新的线程并被线程池管理。
     - **handler：** 拒绝策略，当任务太多来不及处理时，如何拒绝任务。
   - **ThreadPoolExecutor饱和策略：** 

ThreadPoolExecutor的饱和策略是指当任务队列满了并且线程池中的线程数已经达到maximumPoolSize时，采取的处理策略。ThreadPoolExecutor提供了四种饱和策略：
1. AbortPolicy：这是默认的饱和策略。当任务被拒绝时，它会抛出一个未检查的RejectedExecutionException。
2. CallerRunsPolicy：这种策略下，当线程池未关闭且已经饱和，那么会用调用者的线程来执行当前的任务。
3. DiscardPolicy：这种策略直接丢弃任务，不给任何处理和返回。如果允许任务丢失，这是最好的一种方案。
4. DiscardOldestPolicy：丢弃队列中最早的未处理任务，然后尝试再次提交当前任务。这种策略和DiscardPolicy相比，其优点在于，由于要先处理队列中后面的任务，可以使得队列中的任务对应的数据更早的得到处理。

这四种策略都是实现了RejectedExecutionHandler接口。如果这四种策略都无法满足需求，也可以自定义RejectedExecutionHandler接口来处理饱和情况。

   - **线程池工作过程：**
     1. 线程池刚创建时，里面没有一个线程。任务队列是作为参数传进来的。不过，就算队列里面有任务，线程池也不会马上执行它们。
     2. 当调用 execute() 方法添加一个任务时，线程池会做如下判断：
        1. 如果正在运行的线程数量小于 corePoolSize，那么马上创建线程运行这个任务；
        2. 如果正在运行的线程数量大于或等于 corePoolSize，那么将这个任务放入队列；
        3. 如果这时候队列满了，而且正在运行的线程数量小于 maximumPoolSize，那么还是要创建非核心线程立刻运行这个任务；
        4. 如果队列满了，而且正在运行的线程数量大于或等于 maximumPoolSize，那么线程池会抛出异常 RejectExecutionException。
     3. 当一个线程完成任务时，它会从队列中取下一个任务来执行。
     4. 当一个线程无事可做，超过一定的时间（keepAliveTime）时，线程池会判断，如果当前运行的线程数大于 corePoolSize，那么这个线程就被停掉。所以线程池的所有任务完成后，它最终会收缩到 corePoolSize 的大小。

![](/Res/images/Java线程池工作过程.png)
补充：
ThreadPoolExecutor的处理流程应该是：核心线程池已满 --> 等待队列已满 --> 线程池已满 --> ThreadPoolExecutor饱和策略。
详细的处理过程如下：
   - 当一个新的任务提交给ThreadPoolExecutor时，首先会判断线程池中的线程数是否已经达到corePoolSize。如果没有达到，就创建一个新的线程来执行任务。
   - 如果线程池中的线程数已经达到corePoolSize，新提交的任务就会被放到等待队列中。
   - 如果等待队列已经满了，且线程池中的线程数还没有达到maximumPoolSize，就会再创建新的线程来处理任务。
   - 如果线程池中的线程数已经达到maximumPoolSize，此时再有新任务提交，就会触发饱和策略。

![](/Res/images/线程池工作过程.png)
### 3.并发工具类
- **synchronized 关键字：** 方法锁和对象锁，控制同步访问。

在Java中，`synchronized`是一个关键字，用于实现线程间的同步，确保多个线程对共享资源的安全访问。`synchronized`可以用来修饰方法或代码块，以下是关于`synchronized`关键字的一些重要概念：

1. 方法级别的同步：
   - 在Java中，可以使用`synchronized`关键字修饰方法，以实现方法级别的同步。当一个线程进入被`synchronized`修饰的方法时，会自动获取该方法所属对象的锁，其他线程则需要等待锁的释放才能进入该方法。
   - 方法级别的同步适用于整个方法需要同步的情况，但可能会导致性能下降，因为其他线程在等待锁时会被阻塞。

2. 代码块级别的同步：
   - 除了修饰整个方法，`synchronized`还可以用于代码块级别的同步。通过`synchronized`关键字后跟括号中的锁对象来指定需要同步的对象。
   - 使用代码块级别的同步可以避免方法级别同步的性能问题，只对需要同步的部分代码进行同步。

3. 对象锁：
   - 在Java中，每个对象都有一个关联的锁，称为对象锁。当一个线程进入`synchronized`修饰的方法或代码块时，会尝试获取该对象的锁。
   - 当一个线程获取了对象锁后，其他线程需要等待该锁的释放才能进入被`synchronized`修饰的方法或代码块。

4. 类锁：
   - 在Java中，每个类也有一个关联的锁，称为类锁。类锁是由`synchronized`修饰的静态方法或静态代码块所持有的锁。
   - 类锁可以确保对于整个类的静态成员的访问是线程安全的。

5. 重入性：
   - Java中的`synchronized`关键字是可重入的，一个线程可以多次获取同一个对象的锁，而不会发生死锁。这种机制称为重入性。

`synchronized`关键字是Java中实现同步的重要机制，可以确保多个线程对共享资源的安全访问。然而，在一些高并发情况下，`synchronized`的性能可能不如其他同步机制，如`Lock`接口和`java.util.concurrent`包提供的并发工具类。
   - **volatile 关键字：** 保证变量的可见性，避免指令重排序。

`volatile`是Java中的一个关键字，用于声明变量，确保多个线程之间对变量的可见性，即当一个线程修改了被`volatile`修饰的变量的值时，其他线程能够立即看到最新的值。以下是`volatile`关键字的一些重要特点和用法：

1. 可见性：
   - 使用`volatile`修饰的变量可以保证对其他线程的可见性，即当一个线程修改了该变量的值后，其他线程能够立即看到最新的值。
   - 这是因为`volatile`变量的值会被立即刷新到主内存中，并且当一个线程读取`volatile`变量时，会直接从主内存中获取最新的值，而不是从线程的本地缓存中读取。

2. 禁止指令重排序：
   - 使用`volatile`修饰的变量会禁止指令重排序，确保了程序的执行顺序与代码的顺序一致。
   - 这对于一些特定的并发场景非常重要，例如双重检查锁定模式（Double-Checked Locking）中的变量使用`volatile`修饰，可以确保线程安全地获取单例对象。

3. 不保证原子性：
   - 虽然`volatile`可以确保对变量的读取和写入操作的可见性，但并不保证对变量的复合操作的原子性。
   - 如果一个变量的操作需要保证原子性，例如增加或减少一个计数器的值，就需要使用`synchronized`关键字或`java.util.concurrent.atomic`包中的原子类来实现。

4. 适用场景：
   - `volatile`适用于状态标志位等在多线程中需要共享的变量，但不适用于需要原子性操作的场景。
   - 例如，当一个线程修改了一个标志位表示任务已经完成时，其他线程可以通过读取`volatile`变量来感知到这个状态的变化，从而做出相应的处理。

总的来说，`volatile`关键字用于确保多个线程之间对变量的可见性，禁止指令重排序，适用于一些状态标志位等需要共享的场景，但并不保证对变量的复合操作的原子性。
   - **Lock 接口：** ReentrantLock 等具体实现提供比 synchronized 更灵活的锁操作。

`Lock`接口是Java并发包中用于实现锁的一种方式，相比于`synchronized`关键字，它提供了更灵活的锁定和解锁机制。以下是关于`Lock`接口的一些重要特点和用法：

1. 灵活性：
   - `Lock`接口提供了比`synchronized`更灵活的锁定和解锁机制。它允许使用者手动地获取和释放锁，可以在需要时选择性地获取锁、尝试获取锁、定时获取锁等。

2. 条件变量：
   - `Lock`接口提供了支持条件变量（Condition）的能力。条件变量允许线程以分组的方式等待特定的条件发生，从而更灵活地控制线程的等待和唤醒。

3. 可中断性：
   - 与`synchronized`关键字不同，`Lock`接口提供了可中断的锁定操作。即当一个线程等待获取锁时，另一个线程可以通过中断该线程来取消等待，并抛出`InterruptedException`异常。

4. 公平性：
   - `Lock`接口提供了对于锁的公平性设置。在公平模式下，锁将按照请求的顺序分配给等待线程；而在非公平模式下，锁将不保证分配给等待线程的顺序。

5. 替代性：
   - `Lock`接口提供了`synchronized`关键字的替代方案，可以用于实现同步代码块和同步方法的功能。它提供了与`synchronized`关键字类似的功能，但更加灵活和强大。

6. 具体实现：
   - Java并发包中提供了多个具体实现了`Lock`接口的类，包括`ReentrantLock`、`ReentrantReadWriteLock.ReadLock`、`ReentrantReadWriteLock.WriteLock`等，分别用于提供可重入锁、读写锁等不同的锁定机制。

总的来说，`Lock`接口是Java并发包中用于实现锁的一种方式，提供了比`synchronized`更灵活和强大的锁定和解锁机制，支持条件变量、可中断性、公平性等特性。在需要更多控制和灵活性的并发编程场景中，`Lock`接口是一个非常有用的工具。
   - **Atomic 包：** 利用 CAS 操作实现无锁的线程安全编程（如 AtomicInteger）。

`java.util.concurrent.atomic`包提供了一组原子操作类，用于在多线程环境下对变量进行原子操作，保证了操作的线程安全性。以下是关于`java.util.concurrent.atomic`包的一些重要内容：

1. 原子操作类：
   - `java.util.concurrent.atomic`包提供了一系列原子操作类，包括`AtomicBoolean`、`AtomicInteger`、`AtomicLong`等，分别用于对布尔值、整型、长整型等数据类型的原子操作。
   - 这些原子操作类提供了一系列的原子性操作方法，如`get()`、`set()`、`compareAndSet()`等，确保对变量的操作在单个方法调用中完成，并且保证了操作的原子性。

2. 原子更新字段类：
   - 除了基本类型的原子操作类，`java.util.concurrent.atomic`包还提供了一系列原子更新字段类，如`AtomicIntegerFieldUpdater`、`AtomicLongFieldUpdater`等，用于原子地更新对象的字段值。
   - 这些原子更新字段类可以用于更新对象中的整型、长整型等字段的值，并提供了一系列的原子更新方法，如`compareAndSet()`、`getAndIncrement()`等。

3. 原子数组类：
   - `java.util.concurrent.atomic`包还提供了一系列原子数组类，如`AtomicIntegerArray`、`AtomicLongArray`等，用于原子操作数组中的元素。
   - 这些原子数组类提供了一系列的原子操作方法，如`get()`、`set()`、`getAndIncrement()`等，确保对数组元素的操作在单个方法调用中完成，并且保证了操作的原子性。

4. 适用性：
   - `java.util.concurrent.atomic`包中的原子操作类适用于高并发环境下对共享变量进行原子操作的场景。它们提供了一种高效、线程安全的方式来更新变量的值，避免了使用`synchronized`关键字等锁机制带来的性能开销。

5. 注意事项：
   - 虽然`java.util.concurrent.atomic`包提供了原子操作类，但并不是所有的情况都适合使用原子操作类。在某些需要多个操作组合而成的情况下，可能需要考虑使用锁机制来确保操作的原子性。

总的来说，`java.util.concurrent.atomic`包提供了一组原子操作类，用于在多线程环境下对变量进行原子操作，保证了操作的线程安全性，并且提供了一种高效、简单的方式来实现线程安全的变量更新操作。
### 4.并发工具
   - CountDownLatch：允许一个或多个线程等待一系列事件发生。
   - CyclicBarrier：使一定数量的线程到达一个同步点后再一起继续执行。
   - Semaphore：控制同时访问某个资源或操作的线程数量。
   - Exchanger：两个线程在同一点交换数据的同步点。
   - 其它：
     - Fork/Join 框架：用于大任务分解成小任务并行执行，然后合并结果的框架。
     - BlockingQueue：用于生产者消费者模式的阻塞队列接口。
### 5.并发集合
   - **BlockingQueue**

**ArrayBlockingQueue**：数组结构组成的有界阻塞队列，按 FIFO（先进先出）原则对元素进行排序。
**LinkedBlockingQueue**：链表结构组成的有界（但大小默认值为Integer.MAX_VALUE）阻塞队列，按 FIFO 排序元素，吞吐量通常要高于ArrayBlockingQueue。
**PriorityBlockingQueue**：支持优先级排序的无界阻塞队列。
**DelayQueue**：使用优先级队列实现的延迟无界阻塞队列，只有在延迟期满时才能从中提取元素。
**SynchronousQueue**：不存储元素的阻塞队列，也即单个元素的队列，每个插入操作必须等到另一个线程调用移除操作，否则插入操作一直处于阻塞状态，吞吐量通常要高于LinkedBlockingQueue。
**LinkedTransferQueue**：由链表结构组成的无界阻塞 TransferQueue 队列。
**LinkedBlockingDeque**：由链表结构组成的双向阻塞队列。
   - **ConcurrentHashMap**

与Hashtable和synchronizedMap相比，ConcurrentHashMap具有更高的并发性。Hashtable和synchronizedMap在执行更新操作（如put和remove）时，会锁住整个Map，使得任何读/写操作都要等待锁释放，而ConcurrentHashMap则通过对Map进行分段（Segment）来实现更细粒度的锁定，这样理论上允许同时有16个线程对Map进行更新操作（实际并发度取决于更新数据的分布，以及CPU对线程的调度）。

ConcurrentHashMap使用了一种新的方法叫做**锁分段技术**。它使用Segment来代表这些不同的部分，每个Segment其实就是一个抽象的指向一个HashEntry数组的引用。在写操作时，只需要锁住当前段即可，这样就能保证写操作的线程安全性，同时也能保证并发性。

ConcurrentHashMap提供了线程安全的同时，读取操作大部分时候都不需要加锁，因为它只有在读到的值为null或检测到链表有环（可能是其他写线程引起的）时才需要加锁。
总的来说，ConcurrentHashMap是一个用于并发场景下的高效线程安全的HashMap。
   - CopyOnWriteArrayList

   ![](/Res/images/concurrent包.png)
   ![](/Res/images/JUC架构.png)
### 6.并发编程最佳实践

   - 避免共享状态：尽可能使线程不共享状态或者数据。
   - 最小同步策略：尽量减小同步代码块的范围，避免死锁等问题。
   - 不可变对象：使用不可变对象来避免同步问题。

## 四、八股问题整理

### (1).Java中实现线程的方法

1. 继承Thread类
2. 实现Runnable接口
3. 实现Callable接口
4. 利用线程池方式创建

### (2).如何停止一个正在运行的线程  

1. 使用退出标志，使线程正常退出，也就是当run方法完成后线程终止。
2. 使用stop方法强行终止，但是不推荐这个方法，因为stop和suspend及resume一样都是过期作废的方法。
3. 使用interrupt方法中断线程。

### (3).notify和notifyAll有什么区别

1. notify可能会导致死锁，而notifyAll则不会
2. 任何时候只有一个线程可以获得锁，也就是说只有一个线程可以运行synchronized 中的代码
3. 使用notifyall,可以唤醒所有处于wait状态的线程，使其重新进入锁的争夺队列中，而notify只能唤醒一个。
4. wait()应配合while循环使用，不应使用if，务必在wait()调用前后都检查条件，如果不满足，必须调用notify()唤醒另外的线程来处理，自己继续wait()直至条件满足再往下执行。
5. notify() 是对notifyAll()的一个优化，但它有很精确的应用场景，并且要求正确使用。不然可能导致死锁。正确的场景应该是 WaitSet中等待的是相同的条件，唤醒任一个都能正确处理接下来的事项，如果唤醒的线程无法正确处理，务必确保继续notify()下一个线程，并且自身需要重新回到WaitSet中.

### (4).sleep()和wait()有什么区别  

sleep()和wait()都可以用来暂停当前线程的执行，但它们在用法和目的上有一些重要的区别：
1. **属于的类：**
   - sleep()是Thread类的静态方法。
   - wait()是Object类的方法，因为在Java中所有的对象都继承自Object。
2. **锁的处理：**
   - 当线程调用sleep()方法时，它不会释放任何锁。
   - 当线程调用wait()方法时，它会释放该对象的锁，这使得其他线程可以使用synchronized同步块或方法。
3. 唤醒条件：
   - sleep()方法在指定的时间过后会自动唤醒线程。
   - wait()方法通常需要依靠notify()或notifyAll()方法被其他线程唤醒，除非wait()方法有一个超时值。
4. 异常处理：
两者在中断时都会抛出`InterruptedException`异常，但处理方式略有区别。wait()通常与一个条件循环结合使用，以在中断后重新检查条件，而sleep()没有这样的通常约定。
5. 用途：
   - sleep()主要是为了延迟或定时。
   - wait()则用于线程间的通信，等待某个条件满足时再继续执行。

总的来说，sleep()是让线程暂停执行一段时间，不释放对象锁；而wait()是让线程停下来等待某个条件，释放对象锁使得其他线程能够获取锁来改变这个条件。

### (5).volatile是什么？可以保证有序性吗？

volatile是Java语言提供的一个关键字，用作变量声明的修饰符。当一个变量被声明为volatile之后，这个关键字向编译器和虚拟机表明，**这个变量是随时可能被不同的线程修改的**。

**这里是volatile主要的两个功能：**
- **确保内存可见性:**
volatile保证了一个线程对这个变量的写入对于其他线程立即可见。这意味着当一个线程更新了volatile变量的值，这个新值对于其他访问此变量的线程来说是立即可见的。
- **防止指令重排序:**
volatile可以防止指令的重排序优化（它禁止指令之间的重排序，但不保证全局的有序性）。这是因为当操作volatile变量时，会有特定的内存屏障指令来告诉处理器不要进行通常的指令重排优化，从而保证执行顺序上的一致性。

然而，要注意的是，volatile并不是万能的，它能够解决变量的可见性和部分有序性问题，但并不意味着对volatile变量操作的整体行为就是原子性的。就是说，volatile不能保证复合（比如volatileVar++）或者非原子性操作的线程安全。

例如, 一个简单的自增操作`volatileVar++`，虽然读取volatileVar的值是最新的，但自增操作包括读取-修改-写入三个步骤，并不是原子性的。这意味着，如果多个线程并发访问这个变量，那么不适当的同步措施可能会导致不正确的结果。

总之，volatile确实可以在某种程度上保证有序性，它确保了对volatile变量的读写操作在内存中的有序性（即不会被重排序），但它并不是一个锁，不足以替代 synchronized、Lock等同步措施来保证整体动作的原子性，尤其是在多个步骤的操作过程中。

**volatile的应用场景**

1. **标识状态标志**：当一个变量被多个线程共享，并且其中一个线程修改了该变量的值，而其他线程需要立即看到这个变量的最新值时，可以使用`volatile`来声明该变量。比如，用于标识程序是否需要继续运行的标志。

2. **轻量级同步**：`volatile`关键字提供了一种轻量级的同步机制，当访问某个变量时不需要加锁，但仍然需要保证可见性，可以考虑使用`volatile`。但是需要注意，`volatile`不能保证原子性，所以不能替代`synchronized`关键字。

3. **双重检查锁定（Double-Checked Locking）**：在单例模式中，双重检查锁定可以减少同步的开销。在双重检查锁定中，需要使用`volatile`修饰单例对象的引用，以确保在多线程环境下，对单例对象的初始化操作对其他线程可见。

4. **性能调优**：在一些性能敏感的场景中，使用`volatile`可以减少不必要的锁竞争，提高程序的执行效率。

需要注意的是，虽然`volatile`能够保证可见性和禁止指令重排序，但它并不能保证原子性。如果需要保证一系列操作的原子性，仍然需要使用`synchronized`关键字或者`java.util.concurrent`包提供的原子类。

### (6).Thread类中的start()和run方法有什么区别？  

Thread类中的start()方法和run()方法都与线程的执行有关，但它们在功能上有重要的不同：
   - start() 方法:
      - 调用一个线程的start()方法会启动一个新的线程，并使得线程执行它的run()方法。
      - 当你调用start()方法时，Java虚拟机会为这个线程创建新的系统资源，比如一个新的调用栈。
      - start()将启动新线程并立即返回，线程会并行地运行。
   - run() 方法:
      - run()方法包含了线程执行的代码。
      - 如果你直接调用run()方法，它不会启动新线程，而是在当前线程中同步地执行。换句话说，直接调用run()方法会将它当做普通方法的调用，而不会多线程执行。
      - 直接调用线程的run()方法不会给这个线程分配新的调用栈，因此它会影响当前线程的执行流。

因此，当你想启动一个并发执行的线程时，你应该调用start()方法，而不是run()方法。这是因为start()方法会创建一个执行环境，然后在新的执行环境中异步地调用run()方法，而直接调用run()方法并不会产生任何并发行为，它只是对对象上的一个普通方法的调用而已。
### (7).为什么wait、notify、notifyAll这些方法不在Thread类里面？  
- wait(), notify(), 和 notifyAll() 这些方法不在Thread类中，而是在Object类中，原因在于它们和同步锁的监视器 **（Monitor）模式密切相关，而这个模式是基于对象的，而不是线程的。**
- 每个对象在Java中都有一个监视器，用于确保对该对象的同步方法或块的访问是互斥的。当某个线程已经拥有了对象的监视器（即进入了某个同步方法或块）并想要等待某些条件的时候，它就可以调用wait()方法使当前线程进入等待状态。相对应的，notify()或notifyAll()可以被拥有同一个对象监视器的线程调用，以唤醒那些在该对象上等待的线程。
- 如果wait(), notify(), 和 notifyAll()是Thread类的一部分，那么同步将需要显示地关联到特定的线程上，这会使线程间的协作变得复杂且不灵活。相反，将它们设为Object类的一部分使得任何对象都可以作为同步的通信媒介。这样，任何对象都可以充当锁，使线程间的通信更为直接和灵活。
- 例如，当你在设计一个生产者-消费者模式时，wait()和notify()就可以用于控制何时生产者可以生产，消费者可以消费。这些方法的作用是调整和协调多个线程间的同步，而不是直接对线程的操作。
- 这是面向对象设计的一个好例子，展示了把行为（方法）放在可以发挥作用的合适对象（在这里，是任何Java对象）中，而不是局限在明显的地方（比如Thread对象本身）。

明显的原因是JAVA提供的锁是对象级的而不是线程级的，每个对象都有锁，通过线程获得。如果线程需要等待某些锁那么调用对象中的wait()方法就有意义了。如果wait()方法定义在Thread类中，线程正在等待的是哪个锁就不明显了。**简单的说，由于wait，notify和notifyAll都是锁级别的操作，所以把他们定义在Object类中因为锁属于对象**。
### (8).为什么wait和notify方法要在同步块中调用？  

wait()和notify()方法需要在同步块或同步方法中调用，这是由于它们的工作方式需要与对象监视器（monitor）关联。以下是其工作原理的详细解释：
1. 锁和监视器:
   - 线程通过进入同步块或方法获取对象的监视器。同一时刻，只有一个线程可以持有对象监视器，确保线程间的互斥访问。
2. wait():
   - 当线程调用对象的wait()方法时，它会释放当前持有的监视器并等待。这意味着线程暂时停止执行，进入对象的等待集（wait set）。
   - 如果线程未持有相应对象的监视器，它不能调用wait()方法，因为若在没有获取监视器的情况下调用wait()会导致IllegalMonitorStateException。
3. notify() / notifyAll():
   - 调用notify()或notifyAll()也需要线程持有相应对象的监视器，因为它们的功能是唤醒正在等待该对象监视器的其他线程。
   - 如果有多个线程在等待，则notify()随机唤醒一个线程，而notifyAll()唤醒所有等待的线程，它们都需要在重新获取监视器后才能继续执行。
   - 和wait()一样，没有持有相应监视器的线程调用notify()或notifyAll()将抛出IllegalMonitorStateException。

所以，wait()和notify()必须在同步块或同步方法中被调用，以确保逻辑的正确性和程序的线程安全。因为这些方法被设计为解决线程间的协调问题（如消费者-生产者问题），而协调是建立在能够确保对资源互斥访问的基础上的。如果不这么做，便可能导致竞态条件和其他线程安全问题。
### (9).Java中interrupted和isInterrupted方法的区别  
在Java中，对线程中断状态的查询可以通过interrupted()方法和isInterrupted()方法来进行，这两个方法都用于检查线程是否被中断。然而，二者在行为上有以下不同：
   - isInterrupted():
      - 这个方法属于Thread类的实例方法，调用时需要一个线程对象的实例。
      - 它只是检查此线程的中断状态，不会改变中断状态，即调用后中断标志依然保持原状态。
      - 如果线程被中断了，它会返回true；如果没有被中断，它会返回false。
   - interrupted():
      - 这个方法是Thread类的静态方法，可以直接通过类名调用，用来检查当前执行线程的中断状态。
      - 它不仅返回当前线程的中断状态，还会清除中断状态，即如果这个方法被调用，它会返回当前的中断状态，并立即清除中断状态标记（如果它是true的话）。
      - 也就是说，如果一个线程被中断了（中断状态为true），并且你调用了interrupted()方法，第一次会返回true，但是如果你立即再次调用interrupted()，第二次会返回false，因为中断状态已被清除。

因此，如果你需要**检查中断状态**但不想**重置中断标志**，你应该使用isInterrupted()。而如果你需要检查中断状态，并且在检查后重置中断状态，你应该使用interrupted()。通常在处理中断的逻辑时，会根据你是否需要继续响应中断来选择使用哪个方法。
### (10).Java中synchronized 和 ReentrantLock 有什么不同？  
synchronized和ReentrantLock都提供了互斥的功能来控制多线程对共享资源的访问，确保线程安全。但是它们之间存在着一些关键的区别：
1. 锁的管理机制:
   - synchronized是Java内置的同步机制，不需要显示地管理锁的获取和释放，编译器会自动插入锁的请求和释放指令。
   - ReentrantLock是`java.util.concurrent`包的一部分，提供了更灵活的锁操作，需要手动地获取和释放锁（通过lock()和unlock()方法以及try/finally语句块）。
2. 可重入性:
   - 二者都是可重入锁（Reentrant Locks），即同一个线程可以多次获取同一把锁。
3. 锁的公平性:
   - synchronized块默认是非公平锁，不保证等待时间最长的线程将首先获取锁。
   - ReentrantLock则可以设置为公平锁（fairness policy），可以确保按照等待时间的顺序来获取锁。
3. 条件变量:
   - ReentrantLock提供了一个Condition类（通过newCondition()方法创建），允许分割锁，能够使线程有选择地进行等待（await()）及唤醒（signal()或signalAll()），类似于Object类的wait()和notify()方法。
   - synchronized只有单一的内置条件，即与锁关联的对象监视器。
4. 锁的中断处理:
   - 获取ReentrantLock的过程中，线程可以被中断以避免死锁，即在等待锁的过程中可以响应中断。
   - 对于synchronized来说，在等待锁的进程中不能被中断。
5. 尝试获取锁:
   - ReentrantLock提供了tryLock()方法，它可以尝试获取锁，如果锁不可用，该调用会立即返回，不会使线程进入阻塞。
   - synchronized没有直接提供尝试获取锁的功能。
6. 性能:
   - synchronized在JDK 1.6后进行了大量优化，如锁粗化、轻量级锁、偏向锁等，所以性能不再是二者选择的主要差别。
   - 在竞争不激烈的情况下，两者性能相差不大；当锁竞争激烈时，ReentrantLock的性能可能会稍好一些。

总的来说，synchronized简单易用，适合更广泛的场景。而ReentrantLock提供了一些高级功能，适用于需要更复杂锁操作的场景，如公平性选择、条件变量、可中断锁等。开发者应根据需要选择最适合的同步机制。
### (11).有三个线程T1,T2,T3,如何保证顺序执行？ 
1. **使用join()方法**

最简单的方法是在启动一个线程后立即调用它的join()方法，这将会等待线程执行完成后再继续执行下一个线程。
```java
public class SequentialThread {
    public static void main(String[] args) {
        Thread t1 = new Thread(() -> {
            System.out.println("T1 is running.");
        });

        Thread t2 = new Thread(() -> {
            System.out.println("T2 is running.");
        });

        Thread t3 = new Thread(() -> {
            System.out.println("T3 is running.");
        });

        try {
            t1.start();
            t1.join();  // Waits for t1 to die.

            t2.start();
            t2.join();  // Waits for t2 to die.

            t3.start();
            t3.join();  // Waits for t3 to die.
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```
2. **使用wait()和notify()方法**

这些方法可以用来具体控制线程的执行顺序，确保在一个线程完成其工作之前，另一个线程等待。
```java
public class ThreadOrder {
    public static void main(String[] args) {
        final Object lock = new Object();
        
        Thread t1 = new Thread(() -> {
            synchronized (lock) {
                System.out.println("T1 is running.");
                lock.notify();
            }
        });
        
        Thread t2 = new Thread(() -> {
            synchronized (lock) {
                try {
                    lock.wait();
                    System.out.println("T2 is running.");
                    lock.notify();
                } catch (InterruptedException e) {
                    Thread.currentThread().interrupt();
                }
            }
        });
        
        Thread t3 = new Thread(() -> {
            synchronized (lock) {
                try {
                    lock.wait();
                    System.out.println("T3 is running.");
                } catch (InterruptedException e) {
                    Thread.currentThread().interrupt();
                }
            }
        });

        try {
            t3.start();
            t2.start();
            t1.start(); // Start t1 last so it will notify t2 first.
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```
3. **使用ReentrantLock和Condition**


可以使用ReentrantLock锁定一个代码块，并利用它的Condition来阻塞和唤醒线程。
```java
import java.util.concurrent.locks.Condition;
import java.util.concurrent.locks.ReentrantLock;

public class ReentrantSequential {
    private static final ReentrantLock lock = new ReentrantLock();
    
    public static void main(String[] args) throws InterruptedException {
        Condition t1Finished = lock.newCondition();
        Condition t2Finished = lock.newCondition();

        Thread t1 = new Thread(() -> {
            lock.lock();
            try {
                System.out.println("T1 is running.");
                t1Finished.signal();
            } finally {
                lock.unlock();
            }
        });

        Thread t2 = new Thread(() -> {
            lock.lock();
            try {
                t1Finished.await();
                System.out.println("T2 is running.");
                t2Finished.signal();
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            } finally {
                lock.unlock();
            }
        });

        Thread t3 = new Thread(() -> {
            lock.lock();
            try {
                t2Finished.await();
                System.out.println("T3 is running.");
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            } finally {
                lock.unlock();
            }
        });

        t1.start();
        t2.start();
        t3.start();
    }
}
```
4. **使用CountDownLatch**


可以使用CountDownLatch来确保一个线程等待其他线程完成一组操作之后再执行。
```java
import java.util.concurrent.CountDownLatch;

public class LatchSequential {

    public static void main(String[] args) {
        CountDownLatch latch1 = new CountDownLatch(1);
        CountDownLatch latch2 = new CountDownLatch(1);

        Thread t1 = new Thread(() -> {
            System.out.println("T1 is running.");
            latch1.countDown();
        });
        
        Thread t2 = new Thread(() -> {
            try {
                latch1.await();
                System.out.println("T2 is running.");
                latch2.countDown();
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
        });

        Thread t3 = new Thread(() -> {
            try {
                latch2.await();
                System.out.println("T3 is running.");
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
        });

        t1.start();
        t2.start();
        t3.start();
    }
}
```
### (12).SynchronizedMap和ConcurrentHashMap有什么区别？  

SynchronizedMap和ConcurrentHashMap都是Java中的线程安全集合，用于在多线程环境中使用。然而，它们在实现细节和性能上有明显的区别：

**SynchronizedMap**:
   - 它是Collections.synchronizedMap方法返回的包装map，该方法返回一个线程安全的map。
   - 所有的方法调用都是通过在该对象上同步的方式来实现线程安全的。
   - 对于SynchronizedMap中的方法，每次只允许一个线程访问，这意味着即使是不冲突的操作也会导致等待和线程阻塞。
   - 因它的方法都是同步的，所以在高度并发的环境下可能会成为性能瓶颈。
   - 迭代SynchronizedMap是线程安全的，但在迭代期间它必须手动进行同步以防止其他线程的并发修改。

**ConcurrentHashMap**:
   - 它是java.util.concurrent包的一部分，专为高并发环境设计。
   - ConcurrentHashMap使用了锁分段技术（Java 7）和CAS（Compare-And-Swap）操作加上了无锁机制（Java 8），从而允许多个线程同时访问map，增强了并发度。
   - 在ConcurrentHashMap中，不是整个map被锁住，而是某个部分（桶/段）的map被锁，这意味着多个线程可以同时访问不同段的数据，提高了并发性能。
   - Java 8中的ConcurrentHashMap进一步通过使用节点的内部同步和优化的迭代器，减少了锁的使用，提高了性能。
   - 在ConcurrentHashMap的迭代器中做的修改不会直接反映在原始集合中，其视图是弱一致性的。
   - ConcurrentHashMap不允许null键或null值。

总结来说，SynchronizedMap在方法级别上提供同步，适合并发性能需求不高的基本场景。相比之下，ConcurrentHashMap提供细粒度的数据同步，适用于高并发场景，在这些场景中它通常具有更高的性能表现因为它允许并行的读写操作。
### (13).什么是线程安全

线程安全是多线程编程中的一个概念，指某个方法、类、组件或程序能够在多线程环境下被多个线程安全地调用，而不会导致数据被损坏或出现不一致的状态。

1. 以下是线程安全特性的一些主要方面：
   - **数据一致性**：确保共享数据在并发修改的情况下保持正确的状态。
   - **原子性**：操作要么完全执行，要么完全不执行，不能出现执行了一半的情况。
   - **可见性**：一个线程对共享数据的修改能够被其他线程及时看到。
   - **有序性**：确保程序执行的有序性，避免指令重排导致的问题。
   - 例如，一个线程安全的计数器类，能够保证在多个线程对其进行递增操作时，得到正确的计数。
2. 实现线程安全的方法包括：
   - 互斥同步（Mutex）：例如，Java 中的 synchronized 关键字和 ReentrantLock 类通过锁的机制来保证只有一个线程能执行临界区的代码。
   - 非阻塞同步：比如，利用 Atomic 类（如 AtomicInteger）内部的 CAS（Compare-And-Swap）操作实现线程安全。
   - 不变性：创建不可变的对象，因为它们的状态在创建后不能更改，自然就是线程安全的。
   - 线程本地存储：使用 ThreadLocal 类存储数据，实现每个线程都有自己的数据副本，从而避免共享。
   - 并发集合：使用 Java 中的并发集合类，如 ConcurrentHashMap，CopyOnWriteArrayList 等，它们内部已经实现了线程安全。
   - 锁分段技术：例如在 ConcurrentHashMap 中使用，将数据分为几段，每段有自己的锁。

线程安全是并发程序正确性的关键保障，但它可能会带来性能损耗。因此，线程安全的设计应根据实际应用场景合理权衡并发性与性能。  
### (14).Thread类中的yield方法有什么作用？  

yield 方法是 Thread 类的一个静态方法，它的作用是暗示当前正在执行的线程愿意放弃其当前的CPU使用。换句话说，当一个线程调用 yield 方法时，它给线程调度器（属于操作系统的一部分）一个暗示，表明该线程愿意让出CPU给其他的线程。

**但是，需要注意的几点是：**
   - 暗示性质：yield 仅仅是一个暗示，并不保证会产生任何影响。线程调度器完全可以忽视这个暗示，当前的线程仍可能继续执行。
   - 调度策略：具体的行为取决于具体实现的线程调度策略和当前的线程优先级。在某些情况下，同一优先级的其他线程可能获得执行的机会，而在其他情况下，相同或更高优先级的线程可能会被调度。
   - 当前线程状态：调用 yield 的线程将转入可运行（RUNNABLE）状态，而不是等待（WAITING）状态。该线程仍有机会得到CPU时间片，除非有其他线程具有更高的优先级在竞争。
   - 线程优先级：在优先级高的线程调用 yield 比优先级低的线程调用 yield 时，其效果可能不同，因为线程调度通常会考虑优先级。

使用 yield 方法的目的通常是为了提高程序的相对响应性和/或性能，因为它允许其他线程有机会在当前线程不积极执行时更早地执行。然而，在现代多核处理器上，线程调度通常由操作系统内核处理得相当有效，所以 yield 方法的效果可能不像在单核处理器上那么明显。在实际编程中应谨慎使用 yield，因为其在不同操作系统和JVM实现中的行为可能会有很大的差异。

**from《12万字Java面经总结》：**

Yield方法可以暂停当前正在执行的线程对象，让其它有相同优先级的线程执行。它是一个静态方法而且只保证当前线程放弃CPU占用而不能保证使其它线程一定能占用CPU，执行yield()的线程有可能在进入到暂停状态后马上又被执行。
### (15).Java线程池中submit() 和 execute()方法有什么区别？  

在Java的线程池（ExecutorService）中，submit() 和 execute() 方法都可以用来向线程池提交任务以供执行，但它们之间存在一些关键的差异：

**execute() 方法：**
   - execute() 方法接受一个 Runnable 对象，并且没有返回值。
   - 一旦任务开始执行，你就没有办法知道执行结果或者任务是否执行成功。
   - 如果在执行任务时抛出异常，那么异常将被 Thread 的 UncaughtExceptionHandler 捕获或者发送给默认的异常处理器。

**submit() 方法：**
   - submit() 方法可以接受 Runnable 或 Callable 对象。Callable 是一个可以返回值并且可以抛出异常的任务。
   - submit() 方法执行后返回一个 Future 对象，这个对象可以用来检查任务是否执行成功，并且可以通过它来获取 Callable 任务的结果。
   - 如果 submit() 执行的任务抛出异常，这个异常会被 Future 对象捕获，你可以在调用 Future.get() 时得到这个异常。

简而言之，**submit() 方法提供了一种检查任务执行情况和获取返回状态的机制，这是 execute() 方法所不具备的**。选择使用哪一个，主要依赖于你是否需要关注任务的结果。如果你不需要关注结果，并且也不需要处理可能抛出的异常，那么 execute() 方法就足够了；如果你需要任务的返回结果或者需要捕获从任务抛出的异常，你应该使用 submit() 方法。

**from《12万字Java面经总结》：**

两个方法都可以向线程池提交任务，execute()方法的返回类型是void，它定义在Executor接口中,而submit()方法可以返回持有计算结果的Future对象，它定义在ExecutorService接口中，它扩展了Executor接口，其它线程池类像ThreadPoolExecutor和ScheduledThreadPoolExecutor都有这些方法。
### (16).说一说自己对于 synchronized 关键字的了解（重要）

关于 synchronized 关键字，这是一种基于入口锁（Monitor Lock）的同步机制，用于解决多线程环境下的资源竞争问题，确保共享资源在同一时刻只能由一个线程访问，以保障线程安全。
具体来说，synchronized 关键字可以修饰实例方法、静态方法以及代码块：
   - 同步实例方法：通过在方法声明上使用 synchronized 关键字。这样的同步方法会锁定调用该方法的对象实例。
   ```java
      public synchronized void syncMethod() {
         // 方法体
      }
   ```   
   当一个线程访问对象的一个 synchronized 同步方法时，其他线程对该对象的所有其他 synchronized 同步方法的访问将被阻塞。

   - 同步静态方法：通过在静态方法声明上使用 synchronized 关键字。这种方法会锁定这个类的所有对象所关联的 Class 类实例。
   ```java
      public static synchronized void syncStaticMethod() {
         // 方法体
      }
   ```
   静态同步方法锁的是这个类的 Class 对象，它对类的所有实例均有效。

   - 同步代码块：指定锁对象，它可以细化锁定的范围，使得只有在给定对象上加锁。
   ```java
      public void syncBlock() {
         synchronized(this) { // 可以是任意对象，但通常是影响到当前代码段执行的对象
            // 需要被同步的代码
         }
      }
   ```
对象锁就是用作对象监视器。你可以自由指定锁定的对象，这意味着线程只会在锁定对象上同步，不同对象的同步代码块之间不会相互影响。

**工作原理与特性：**

   - 互斥性：synchronized 保证只有拥有对象监视器（锁）的线程可以执行同步代码，其它线程都需要等待锁的释放才有机会执行。
   - 内存可见性：synchronized 保证线程释放锁之前，对共享变量的更改将会被刷新到主内存中，且锁获取之后，线程将清空本地内存，并从主内存中重新读取共享变量。
   - 可重入性：在 Java 中，锁是可重入的。这意味着如果一个 Java 线程进入了代码中的 synchronized 方法或者代码块，并且此时在同步控制的块内部调用了另外的 synchronized 方法，那么该线程可以直接进入该方法而不再需要获得锁。

**注意事项：**
   - 使用 synchronized 锁定的对象应当是不变的引用，以保证锁的唯一性。
   - 对于数据的写操作应选择合适的锁策略，以保证线程安全。
   - synchronized 不会自动解决所有的线程安全问题，例如，它不解决死锁问题。
   - synchronized 相比于 java.util.concurrent 包下的类，如 ReentrantLock，它的功能较为基础且灵活性较低。例如，ReentrantLock 支持尝试非阻塞地获取锁、尝试在给定时间内获取锁以及公平锁等更高级的锁操作。

总之，synchronized 是处理并发程序中同步问题的一个基本工具，通过控制线程的访问权限，来保证数据的一致性和完整性。不过，随着 Java 并发包中工具类的不断丰富，我们也应该根据实际情况，选择更合适的并发控制工具。
### (17).说说自己是怎么使用 synchronized 关键字？ 
**from《12万字Java面经总结》**

- 修饰实例方法: 
  作用于当前对象实例加锁，进入同步代码前要获得当前对象实例的锁 
- 修饰静态方法:
   也就是给当前类加锁，会作用于类的所有对象实例，因为静态成员不属于任何一个实例对象，是类成员（ static 表明这是该类的一个静态资源，不管new了多少个对象，只有一份）。所以如果一个线程A调用一个实例对象的非静态 synchronized 方法，而线程B需要调用这个实例对象所属类的静态 synchronized 方法，是允许的，不会发生互斥现象，因为访问静态 synchronized 方法占用的锁是当前类的锁，而访问非静态 synchronized 方法占用的锁是当前实例对象锁。 
- 修饰代码块: 指定加锁对象，对给定对象加锁，进入同步代码库前要获得给定对象的锁。 

总结： synchronized 关键字加到 static 静态方法和 synchronized(class)代码块上都是是给 Class 类上锁。synchronized关键字加到实例方法上是给对象实例上锁。尽量不要使用 synchronized(String a) 因为JVM中，字符串常量池具有缓存功能！
### (18).什么是线程安全？Vector是一个线程安全类吗？  

线程安全是指当多个线程同时访问某个类（对象或方法）时，这个类始终都能表现出正确的行为，不会因为线程的争用导致数据被破坏或出现非预期的结果。

要实现线程安全，一个类或方法至少要满足以下三个基本要素：
   - 原子性：保证一个操作或多个操作要么完全执行，要么完全不执行，不会出现中间状态。原子操作在执行的过程中不会被其他线程干扰。
   - 可见性：一个线程对共享资源的修改，其他线程能够立即知晓这种修改。在 Java 中，可以通过使用 synchronized 或者 volatile 关键字来保证可见性。
   - 有序性：即程序执行的顺序按照代码的先后顺序执行。

在 Java 集合中，Vector 是一个线程安全类，因为它的许多方法是通过 synchronized 关键字来同步的。比如，它的 add(), get(), remove() 等主要方法都是同步方法，确保对 Vector 实例的单个操作是线程安全的。

不过，需要注意的是，单个的线程安全操作并不能保证组合操作的线程安全性。例如，若有检查然后执行的操作（如检查某个值是否存在，如果不存在则添加），这种组合操作在多线程环境下仍是需要额外的同步措施，尽管 Vector 的单个操作是线程安全的。

Vector 的线程安全特性是在较早的 Java 版本中引入的，随着 Java 的发展，一些新的线程安全集合类被引入，如 ConcurrentHashMap, CopyOnWriteArrayList 等，它们提供了更好的并发性能和更灵活的操作，因此在实际的开发中通常会优先选择这些新的线程安全集合类。
### (19).synchronized关键字和volatile关键字的区别

`synchronized`关键字和 `volatile` 关键字是两个互补的存在，而不是对立的存在！

- volatile 关键字是线程同步的轻量级实现，所以 volatile 性能肯定比 synchronized 关键字要好。但是 volatile 关键字只能用于**变量**而synchronized关键字可以修饰**变量、方法以及代码块**。
- volatile 关键字能保证**数据的可见性**，但不能保证**数据的原子性**。synchronized 关键字两者都能保证。
- volatile 关键字主要用于解决**变量在多个线程之间的可见性**，而 synchronized 关键字解决的是**多个线程之间访问资源的同步性**。
- volatile本质是在告诉jvm当前变量在寄存器（工作内存）中的值是不确定的，需要从主存中读取；synchronized则是锁定当前变量，只有当前线程可以访问该变量，其他线程被阻塞住。
- volatile不会造成线程的阻塞；synchronized可能会造成线程的阻塞。
- volatile标记的变量不会被编译器优化；synchronized标记的变量可以被编译器优化。
### (20).常用的线程池有哪些？ 

Java中提供了多种线程池，主要通过java.util.concurrent.ExecutorService接口及其实现提供。最常用的线程池主要包括：
   - **FixedThreadPool**：拥有固定线程数量的线程池。当线程池的大小达到最大时，会在队列中等待任务的运行。如果某个线程执行完毕，线程池会补充一个线程。
   - **CachedThreadPool**：一个可以根据需要创建新线程的线程池，但会在之前构建的线程可用时重用它们。对于执行很多短期异步任务的程序，这些线程池通常可以提高程序性能。调用execute将重用以前构造的线程（如果线程可用）。如果现有线程没有可用的，则创建一个新线程并添加到池中。未使用的线程会被保持60秒，之后则被终止和移出缓存。
   - **SingleThreadExecutor**：单个后台线程的Executor，创建单个工作线程来执行任务。如果这个唯一的线程因异常结束，会有一个新的线程来替代它执行后续的任务。这保证了任务队列中的任务的顺序执行。
   - **ScheduledThreadPool**：具备定时定期执行任务功能的线程池。核心线程数固定，而非核心线程数无限制，闲置非核心线程即被回收。
   - **SingleThreadScheduledExecutor**：功能类似于ScheduledThreadPool，但它的核心线程数是1，保证所有定时任务都在同一个线程中执行，互不影响。
   - **WorkStealingPool（JDK 1.8 引入）**：内部采用“工作窃取”算法的线程池。它维护了一个任务队列，工作线程可以从其他队列中窃取任务来执行，从而优化了线程的工作，减少了线程闲置的可能性。

**当选择线程池时，应考虑：**
   - 任务的性质（CPU密集型、IO密集型、混合型）
   - 任务所需执行时间（长/短）
   - 预期的同时并发任务数
   - 资源限制等因素

上述线程池通过Executors类中的静态工厂方法很容易创建，但在实际应用中，我们可能需要根据具体的应用场景创建ThreadPoolExecutor的实例来更精细地控制线程池的行为，比如线程池中线程数量、线程存活时间、任务队列类型等。

**from《12万字Java面经总结》：**
   - newSingleThreadExecutor：创建一个单线程的线程池，此线程池保证所有任务的执行顺序按照任务的提交顺序执行。
   - newFixedThreadPool：创建固定大小的线程池，每次提交一个任务就创建一个线程，直到线程达到线程池的最大大小。
   - newCachedThreadPool：创建一个可缓存的线程池，此线程池不会对线程池大小做限制，线程池大小完全依赖于操作系统（或者说JVM）能够创建的最大线程大小。
   - newScheduledThreadPool：创建一个大小无限的线程池，此线程池支持定时以及周期性执行任务的需求。
   - newSingleThreadExecutor：创建一个单线程的线程池。此线程池支持定时以及周期性执行任务的需求。
### (21).什么是线程池？简述一下你对线程池的理解  

线程池是一种用于并行执行多个任务的线程使用模式，在需要执行大量异步任务时非常有用。其基本思想是预先创建一定数量的线程，放入空闲队列中，这些线程都是处于待命状态。当有新任务到来时，不是每次创建新线程，而是从池中取出一个空闲线程来执行任务。执行完毕的线程不会被销毁，而是再次返回到线程池中，等待执行下一次任务。

**线程池主要解决两个问题：**
   - 性能问题：频繁地创建和销毁线程需要时间，如果任务很多但执行时间很短，线程的创建和销毁可能比任务执行耗费更多时间。使用线程池可以避免这种情况，因为创建的线程会被重复利用，减少了创建和销毁线程的开销。
   - 资源消耗问题：每个线程都需要占用系统资源，如果无限制地创建线程会导致系统资源迅速耗尽，可能会使系统变得不稳定。线程池能够有效控制线程的最大并发数，超出的任务则排队等候，这样能够有效管理系统资源。

**线程池的关键参数包括：**
   - 核心线程数（corePoolSize）：线程池中默认的线程数量。
   - 最大线程数（maximumPoolSize）：线程池允许创建的最大线程数。
   - 空闲线程存活时间（keepAliveTime）：非核心线程空闲时的存活时间。
   - 任务队列（workQueue）：用于存放待执行任务的阻塞队列。
   - 线程工厂（threadFactory）：创建新线程的工厂。
   - 拒绝策略（handler）：当任务队列满了且达到最大线程数时如何处理新任务。

**正确地使用线程池能够带来的好处：**
   - 降低资源消耗
   - 提高响应速度
   - 提高线程的可管理性

但也存在一些潜在问题，例如资源耗尽（线程和系统资源）、内存泄漏（由于线程池的不当使用）、性能瓶颈（如果所有线程都在执行长时间任务将引起延迟）等，所以要合理配置线程池参数，并对其生命周期进行管理。
### (22).Java程序是如何执行的  

![](/Res/images/Java程序运行过程.png)

Java程序的执行涉及到几个重要的步骤和组件：
   - 编写源代码：首先，您需要使用文本编辑器编写Java源代码，该代码包含了.java扩展名的文件。
   - 编译源代码：使用Java编译器（javac命令）将.java文件编译成字节码文件。这些字节码文件具有.class扩展名，并且可以在任何安装了Java运行时环境(JRE)的机器上运行。
   - 加载字节码：当执行Java程序时，Java虚拟机(JVM)会启动。JVM首先将.class文件加载到运行时数据区。
   - 字节码验证：载入的字节码在执行之前会进行验证，确保字节码的格式正确，且不会违反JVM的安全限制。
   - 执行：在字节码验证后，JVM的解释器会逐条将字节码翻译为机器码并执行。这个解释执行的过程会有一定的性能开销。为了优化性能，现代JVM提供了即时编译器(JIT)，它可以将热点代码（经常执行的代码）编译为本地机器码以提高效率。
   - 运行时：Java程序运行在JVM的管理之下，JVM负责内存管理、垃圾回收、线程管理等运行时的各种任务。
   - 程序的交互：程序可以通过命令行参数、图形界面、网络或文件系统等多种方式和用户或其他系统交互。

这个过程是Java 平台独立性的关键所在。JVM作为抽象的计算机, 可以在任何具有相应JVM实现的操作系统上运行编译后的Java字节码。因此，Java程序通常可以不经修改地在多种硬件和操作系统平台上运行，实现了“一次编写，到处运行”（Write Once, Run Anywhere，WORA）的理念。
### (23).锁的优化机制了解吗？  

Java提供了多种锁的优化机制，以提高多线程程序的性能。这里主要介绍几种：
1. **偏向锁（Biased Locking）**：偏向锁是Java 6引入的一种锁优化手段。它是一种针对加锁操作的优化策略，有助于消除无竞争同步的部分执行路径。如果一个锁是偏向模式，那么在无竞争的情况下，后续的锁定\解锁操作无需进行任何同步操作，从而消除了这部分操作的开销。但需注意，在出现锁竞争的情况时，偏向锁会增加额外的系统开销。
2. **轻量级锁（Lightweight Locking）**：轻量级锁主要针对少量的竞争情况，如果一个锁是轻量级锁，那么在没有竞争的情况下，使用CAS操作（Compare And Swap 比较与交换）来将锁对象的标记字段设置为锁定状态。如果CAS操作成功，则表示获得了锁；若CAS失败，则表示其他线程同时申请锁，就可以升级为重量级锁。
3. **自旋锁（Spin Lock）**：自旋锁是指当一个线程访问某个共享资源时，如果发现资源已经被其他线程锁住，该线程不立即阻塞自己等待，而是不断地在循环之内检测锁是否被释放。这是一种避免线程阻塞的锁，它的主要思想是：宁愿多消耗CPU资源，也不让线程进入阻塞状态，减少线程阻塞和唤醒的性能损耗。
4. **锁消除（Lock Elimination）**：通过逃逸分析确定一些锁操作是不必要的，然后消除这些不必要的锁操作。
5. **锁粗化（Lock Coarsening）**：将多个连续的锁合并为一个较大范围的锁，减少申请和释放锁带来的性能消耗。

这些优化主要都是针对synchronized关键字而言的，在并发并不高但对性能敏感的场景下，这些优化可以帮助减少不必要的开销，提高程序效率。然而在高并发情况下，开发者可能需要使用更为显式和底层的并发工具，例如java.util.concurrent包中的ReentrantLock等工具和机制。

### (24).说说进程和线程的区别？ 

线程和进程是操作系统中执行任务的两个基本单元。它们之间有一些关键的区别：
1. 定义：
   - 进程：进程是操作系统分配资源和调度的独立单位。每个进程都有自己的一套虚拟内存空间，以及跟踪其运行状态所需的信息。
   - 线程：线程是进程中的一个实体，被系统独立调度和分派的基本单位。线程自身基本上不拥有系统资源，它与同属一个进程的其他线程共享进程的资源。
2. 资源分配和共享：
   - 进程：拥有独立的内存地址空间，一个进程崩溃后，在保护模式操作系统中不会影响其他进程。除了共享文件等资源外，进程间通信(IPC)需要特定的机制来实现。
   - 线程：线程共享其所属进程的内存和资源。线程之间可以直接读写进程数据段（如全局变量）来进行通信。
1. 启动和上下文切换的速度：
   - 进程：比线程慢。每个新进程都需要一定的时间来创建和销毁。
   - 线程：速度比进程快，因为线程之间共享部分环境，所以上下文切换速度较快。
2. 通信方式：
   - 进程：进程间通信需要较复杂的方式，如sockets、共享内存、信号量等。
   - 线程：由于共享内存和数据，线程之间的通信更为容易。
1. 系统开销：  
   - 进程：创建或撤销进程时，由于需要分配或回收资源，所以其系统开销较大。
   - 线程：由于基本没有资源分配的开销，线程的创建、撤销和切换的系统开销远低于进程。
1. 独立性：
   - 进程：是独立运行和资源分配的基本单位。
   - 线程：不独立，是进程的一个实体，依赖于进程的环境。
1. 安全性：
进程：由于拥有独立的地址空间，一个进程不会对另一个进程产生影响。
线程：一个线程死掉可能会影响到相同进程的其他线程。

总的来说，进程与线程的根本区别在于它们对系统资源的分配方式不同。进程有独立的资源，而线程共享资源。在多核处理器上，多线程能够更好地提升应用程序的性能，因为线程间的切换和通信比进程间的代价要小。
### (25).产生死锁的四个必要条件？  
1. **互斥**

指系统中的资源是不可共享的，每个资源在任意时刻只能由一个线程使用。如果其他线程请求该资源，请求者只能等待，直到资源的占有者线程使用完毕释放。 

2. **不可剥夺**

指资源不能被强行从一个线程中剥夺，只能由占有它的线程在使用完毕后自愿释放。只有资源的持有者可以释放该资源。

3. **请求与保持**

指线程至少持有一个资源，并且正在等待获取额外的资源，而该资源可能被其他已经持有其他资源的线程占有。

4. **循环等待**

指在发生死锁时，必然存在一个线程—资源的环形链，每个线程持有一个资源，并等待获取下一个线程所持有的资源。这构成了一个循环等待的资源链。
### (26).如何避免死锁？  

1. **预防死锁**
   - 破坏产生死锁的四个必要条件
2. **避免死锁**
   - **资源分配策略（银行家算法）**：利用银行家算法等避免算法的策略预先计算资源分配序列，只允许系统进入安全状态。在这个状态中，系统可以根据某种顺序分配所需的所有资源给每个线程，而不会进入死锁。
   - **顺序资源分配**：系统中的所有资源类型都按照某种顺序编号。所有线程都必须根据编号的顺序来请求资源。这可以防止循环等待条件的发生，因此也就避免了死锁。
   - **一次性分配资源**：线程在开始执行之前一次性请求所有必需的资源。只有当所有请求立即得到满足时，线程才开始执行。这有可能导致资源的低效使用，但它可以有效地避免死锁。
   - **资源的有限申请**：限制线程申请资源的数量。当线程持有一些资源而请求更多资源时，必须首先释放它已经持有的资源。
   - **不可剥夺资源**：剥夺是指某个线程占有一些资源但在使用之前需要更多资源，而这些资源又被其他线程占有。在这种情况下，系统可以剥夺该线程已占有的资源并分配给其他线程。这样，它就不会继续等待而可以释放资源。
   - **超时机制**：为线程获取资源的操作设置超时时间。一旦超时，线程将释放其拥有的所有资源并重试，或者进行其它的错误处理操作。
3. **检测死锁**
   - 资源分配图：在资源分配图中，节点代表进程和资源，边代表请求和分配。通过分析资源分配图中是否存在环，可以检测到死锁。如果图中存在至少一个环，则系统处于死锁状态。
   - 死锁检测算法：像银行家算法这样的死锁避免算法也可以用来检测死锁。该算法试图找出一个安全序列，如果找不到，则表明系统可能已经进入死锁状态。
   每次资源请求的检查：在处理资源请求时，可以实施检查，以查看授予这些请求是否可能导致死锁。这通常要求较为复杂的算法来确定潜在的依赖关系。
   - 定时检测：有些操作系统通过定时运行一个死锁检测例程来检测死锁。这个例程将检查所有进程和资源，以查看是否存在死锁条件。
   - 等待图：使用等待图（是资源分配图的一个变种），在这个图中，进程是节点，进程等待其他进程释放资源的关系是边。如果图中存在环，那么表明发生了死锁。
4. **解决死锁**
   - 资源剥夺：暂时剥夺某个进程的资源，并将其分配给其他进程。待其他进程完成任务后，再将资源返回给被剥夺资源的进程。这种方法可能导致剥夺资源的进程需要重新计算，或者至少回退到某个安全点再重新开始。
   - 进程回滚：将一个或多个进程回滚到它们之前的某个状态，特别是回滚到可以避免死锁的状态。这种方法需要预先有进程状态保存的机制，通常要考虑到回滚开销和实现的复杂性。
   - 进程终止：强行终止处于死锁的一个或多个进程。终止进程可以按照某种顺序进行，例如，优先终止最少代价的进程，或者一次性终止所有进程。这种方法存在的风险是可能导致未保存的进程状态丢失。
   - 资源重新分配：动态地调整资源分配，以打破死锁状态。这可能涉及到将一些低优先级的任务延迟，以确保高优先级的任务能够继续执行。

解决死锁问题时，需要衡量每种方法的副作用，如资源剥夺可能导致进程性能下降，进程回滚和终止可能会导致数据丢失或者不一致。因此，在实际情况中可能需要混合使用以上方法，或者根据不同场景选择最合适的策略。通常，解决死锁的策略会结合适当的预防和避免措施，以减少死锁的发生概率，并减轻解决死锁所需的开销。
### (27).线程池核心线程数怎么设置呢？  

from《Java12万字面经总结》：
分为CPU密集型和IO密集型
- **CPU密集型：**
这种任务消耗的主要是 CPU 资源，可以将线程数设置为 N（CPU 核心数）+1，比 CPU 核心数多出来的一个线程是为了防止线程偶发的缺页中断，或者其它原因导致的任务暂停而带来的影响。一旦任务暂停，CPU 就会处于空闲状态，而在这种情况下多出来的一个线程就可以充分利用 CPU 的空闲时间。
- **IO密集型：**
这种任务应用起来，系统会用大部分的时间来处理 I/O 交互，而线程在处理 I/O 的时间段内不会占用 CPU 来处理，这时就可以将 CPU 交出给其它线程使用。因此在 I/O 密集型任务的应用中，我们可以多配置一些线程，具体的计算方法是 ： 核心线程数=CPU核心数量*2。
### (28).Java线程池中队列常用类型有哪些?(阻塞队列)

- **ArrayBlockingQueue**：数组结构组成的有界阻塞队列，按 FIFO（先进先出）原则对元素进行排序。
- **LinkedBlockingQueue**：链表结构组成的有界（但大小默认值为Integer.MAX_VALUE）阻塞队列，按 FIFO 排序元素，吞吐量通常要高于ArrayBlockingQueue。
- **PriorityBlockingQueue**：支持优先级排序的无界阻塞队列。
- **DelayQueue**：使用优先级队列实现的延迟无界阻塞队列，只有在延迟期满时才能从中提取元素。
- **SynchronousQueue**：不存储元素的阻塞队列，也即单个元素的队列，每个插入操作必须等到另一个线程调用移除操作，否则插入操作一直处于阻塞状态，吞吐量通常要高于LinkedBlockingQueue。
- **LinkedTransferQueue**：由链表结构组成的无界阻塞 TransferQueue 队列。
- **LinkedBlockingDeque**：由链表结构组成的双向阻塞队列。
选择哪种类型的队列，取决于具体的业务需求和系统资源。建议在理解每种队列特性和业务需求后，再做出合适的选择。
### (29).线程安全需要保证几个基本特征？  

1. **原子性**，简单说就是相关操作不会中途被其他线程干扰，一般通过同步机制实现。
2. **可见性**，是一个线程修改了某个共享变量，其状态能够立即被其他线程知晓，通常被解释为将线程本地状态反映到主内存上，volatile 就是负责保证可见性的。
3. **有序性**，是保证线程内串行语义，避免指令重排等。
### (30).说一下线程之间是如何通信的？  

线程之间的通信有两种方式：共享内存和消息传递。
1. **共享内存**
   - 在共享内存的并发模型里，线程之间共享程序的公共状态，线程之间通过写-读内存中的公共状态来隐式进行通信。典型的共享内存通信方式，就是通过共享对象进行通信。
   - 例如上图线程 A 与 线程 B 之间如果要通信的话，那么就必须经历下面两个步骤：
     1. 线程 A 把本地内存 A 更新过得共享变量刷新到主内存中去。
     2. 线程 B 到主内存中去读取线程 A 之前更新过的共享变量。
2. **消息传递**
在消息传递的并发模型里，线程之间没有公共状态，线程之间必须通过明确的发送消息来显式进行通信。在 Java 中典型的消息传递方式，就是 wait() 和 notify() ，或者 BlockingQueue 。
### (31).CAS的原理呢？  
CAS（Compare and Swap）是一种**无锁的原子操作**，它的全称是“比较并交换”。主要应用在多线程编程中，用于解决多线程并发时数据一致性的问题。

- CAS操作包含三个参数：内存位置（V）、预期原值（A）和新值（B）。如果内存位置的值与预期原值相匹配，那么将内存位置的值修改为新值。否则，不做任何操作。一般情况下，整个操作是一个原子操作。
- CAS的基本工作原理是，当线程需要改变共享数据时，先读取数据的当前值A，然后通过某种计算得到新的值B，此时，系统会比较当前值A是否与读取时获取的值A一致，如果一致，就把新值B写入，如果不一致，说明有其他线程修改了数据，则需要重新读取数据并计算新的值。
- CAS有一个重要的使用场景就是实现线程安全的计数器。Java的原子包java.util.concurrent.atomic下的原子变量类就是通过CAS来实现的。
- 需要注意的是，虽然CAS解决了原子操作问题，但它也有三个主要的问题：循环时间长、只能保证一个共享变量的原子操作、和ABA问题。
### (32).CAS有什么缺点吗？  

CAS（Compare And Swap）虽然在多线程编程中起到了很大的作用，但是它也存在以下几个主要的缺点：

- **ABA问题**：这是CAS的一个经典问题。因为CAS需要在操作值的时候检查内存值是否发生变化，没有发生变化则更新，但是如果一个值原来是A，变成了B，然后又变成了A，那么使用CAS进行检查时会发现没有发生变化，但是实际上却发生了变化。这就是ABA问题。解决ABA问题可以使用版本号。在Java中，原子包java.util.concurrent.atomic下的类AtomicStampedReference和AtomicMarkableReference就是通过版本号或者标记的方式来解决ABA问题。
- **循环时间长开销大**：CAS操作失败会进行一直进行尝试，如果一直不成功，会对CPU造成较大的执行开销。
- **只能保证一个共享变量的原子操作**：当对一个共享变量执行操作时，CAS能够保证操作的原子性，但是对于多个共享变量的操作，CAS就无法保证操作的原子性。这个时候，就可以用锁，或者利用把多个共享变量合并成一个共享变量来操作（如使用版本号）。

以上就是CAS的主要缺点，虽然有这些缺点，但在很多情况下，CAS仍然是一种非常有用的非阻塞算法。
### (33).引用类型有哪些？有什么区别？  

在Java中，引用类型主要有四种，分别是：强引用（Strong Reference）、软引用（Soft Reference）、弱引用（Weak Reference）和虚引用（Phantom Reference）。它们的区别主要体现在垃圾回收的行为上。
1. **强引用（Strong Reference）**：这是使用最普遍的引用。如果一个对象具有强引用，那就类似于 "Object obj = new Object()" 这类的引用，只要强引用还存在，垃圾收集器永远不会回收被引用的对象。
2. **软引用（Soft Reference）**：软引用通过SoftReference类实现。软引用的生命周期比强引用短一些。只有当 JVM 认为内存不足时，才会去试图回收软引用指向的对象：即 JVM 会确保在抛出 OutOfMemoryError 之前，清理软引用指向的对象。
3. **弱引用（Weak Reference）**：弱引用通过WeakReference类实现。弱引用的生命周期比软引用更短。在垃圾回收器线程扫描它所管辖的内存区域的过程中，一旦发现了弱引用对象，不管当前内存空间足够与否，都会回收它的”所指向的对象“。
4. **虚引用（Phantom Reference）**：虚引用主要用来跟踪对象被垃圾回收的活动，虚引用无法决定对象的生命周期。如果一个对象仅持有虚引用，那么它就和没有任何引用一样，在任何时候都可能被垃圾回收。虚引用主要用于检测被对象回收的事件。

### (34).说说ThreadLocal原理？ 
ThreadLocal是Java中的一个类，它提供了一种将对象绑定到当前线程的机制。在多线程环境下，通过ThreadLocal创建的变量只能被当前线程访问，其他线程则无法访问和修改。

ThreadLocal的工作原理如下：
- ThreadLocal类内部有一个Map，用于存储每一个线程的变量副本。
- 当你创建一个ThreadLocal变量后并调用set()方法，实际上是往ThreadLocal内部的Map中存入数据，其中键是当前线程，值是set()方法传入的参数。
- 当调用get()方法获取ThreadLocal变量时，实际上是从ThreadLocal内部的Map中取数据，其中的键是当前线程。

这样，每一个线程都可以独立地改变自己的副本，而不会影响其他线程所对应的副本，从而实现了线程隔离，提升了多线程的并发性。ThreadLocal常常用于实现线程内的数据共享，如数据库连接、Session管理等。
![](/Res/images/ThreadLocal原理.png) 
### (35).线程池原理知道吗？以及核心参数  
线程池是多线程处理中常用的一种处理模式，**线程池的工作原理是线程复用，控制系统资源的消耗，提高系统响应速度。**

线程池的工作原理是预先创建一些线程放入一个池子（也就是队列）中，这些线程都是处于休眠状态，也就是空闲状态。当有任务提交时，从池子中取出一个线程去执行这个任务，执行完该任务的线程并不会被销毁，而是再次返回到池子中等待下一次使用。

**线程池的核心参数主要有以下几个：**
- **corePoolSize：** 核心线程数，线程池创建时候，线程的数量，当线程池中的线程数目达到corePoolSize后，就会把到达的任务放到队列中。
- **maximumPoolSize：** 线程池最大线程数，这个参数也就是线程池能够容纳同时执行的最大线程数，超出的线程会被队列缓存。
- **workQueue：** 任务队列，被提交但尚未被执行的任务。
- **keepAliveTime：** 线程空闲时间，当线程空闲时间达到keepAliveTime后，线程会被销毁，直到只剩下corePoolSize个线程为止。
- **unit：** 时间单位，keepAliveTime的单位。
- **threadFactory：** 线程工厂，用于创建新的线程并被线程池管理。
- **handler：** 拒绝策略，当任务太多来不及处理时，如何拒绝任务。

### (36).线程池的拒绝策略（ThreadPoolExecutor饱和策略）有哪些？  
1. AbortPolicy：直接丢弃任务，抛出异常，这是默认策略
2. CallerRunsPolicy：只用调用者所在的线程来处理任务
3. DiscardOldestPolicy：丢弃等待队列中最旧的任务，并执行当前任务
4. DiscardPolicy：直接丢弃任务，也不抛出异常
### (37).说说你对JMM内存模型的理解？为什么需要JMM？  
### (38).多线程有什么用？为什么要使用多线程  

1. 提高CPU的利用率：多线程可以使得多个任务并发执行，当一个线程等待IO操作完成时，CPU可以切换到其它线程继续执行计算操作，使得CPU资源得到充分利用。
提高应用程序的响应性：通过多线程，可以使得一部分线程执行耗时的操作，而其他线程保持对用户输入的响应。
2. 简化复杂的异步操作：在一些需要处理复杂异步操作的场景中，使用多线程可以简化程序的设计和开发。
3. 利用多核处理器：现代计算机大部分都是多核处理器，多线程能让多个线程分散到不同的处理器（或核）上执行，从而提高程序的执行效率。

例如，在Web服务器中，通常会为每一个请求分配一个线程进行处理，这样可以同时处理多个客户请求，提高服务器的吞吐量。在一些复杂的图形处理、科学计算中，也常常使用多线程来提高处理速度。
### (39).说说CyclicBarrier和CountDownLatch的区别？ 

CyclicBarrier和CountDownLatch都是用于线程同步的工具类，它们都能实现让一组线程等待至某个状态后再全部同时执行，但是它们的使用场景和工作方式有所不同。

- **CyclicBarrier**：CyclicBarrier字面意思是可循环使用（Cyclic）的屏障（Barrier）。它要求固定数量的线程（parties）必须在屏障处等待，直到所有线程都到达屏障处，屏障才会打开，所有线程才能继续执行。CyclicBarrier是可以重用的，一旦屏障打开，如果需要，可以再次在屏障处等待。
- **CountDownLatch**：CountDownLatch是一个同步工具类，它允许一个或多个线程等待直到在其他线程中执行的一组操作完成。它的工作原理是，CountDownLatch一个非负计数器，调用countDown()方法就将计数器减1，计数器的值变为0时，因调用await()方法被阻塞的线程就会被唤醒。注意CountDownLatch是不能被重用的。

总结来说，主要区别在于：
- CyclicBarrier的重点是，所有线程必须同时到达屏障点，才能继续执行，而 CountDownLatch则是等待其他线程执行完毕，自己才继续执行。
- CyclicBarrier是可以重用的，CountDownLatch则一旦计数器值变为0，就不能再用了。

**from《12万字Java面经总结》：**
1. CyclicBarrier的某个线程运行到某个点上之后，该线程即停止运行，直到所有的线程都到达了这个点，所有线程才重新运行；CountDownLatch则不是，某线程运行到某个点上之后，只是给某个数值-1而已，该线程继续运行
2. CyclicBarrier只能唤起一个任务，CountDownLatch可以唤起多个任务
3. CyclicBarrier可重用，CountDownLatch不可重用，计数值为0该CountDownLatch就不可再用了

**from《JavaGuide》：**
CountDownLatch （倒计时器）： CountDownLatch 是⼀个同步工具类，⽤来协调多个线
程之间的同步。这个工具通常用来控制线程等待，它可以让某⼀个线程等待直到倒计时结
束，再开始执⾏。
CyclicBarrier (循环栅栏)： CyclicBarrier 和 CountDownLatch 非常类似，它也可以实现线程间的技术等待，但是它的功能比 CountDownLatch 更加复杂和强大。主要应用场景和CountDownLatch 类似。 CyclicBarrier 的字面意思是可循环使用（ Cyclic ）的屏障（ Barrier ）。它要做的事情是，让⼀组线程到达⼀个屏障（也可以叫同步点）时被阻塞，直到最后⼀个线程到达屏障时，屏障才会开门，所有被屏障拦截的线程才会继续干
活。 CyclicBarrier 默认的构造方法是 CyclicBarrier(int parties) ，其参数表示屏障拦截的线程数量，每个线程调用 await() 方法告诉 CyclicBarrier 我已经到达了屏障，然后当前线程被阻塞。
### (40).什么是AQS？
AQS（AbstractQueuedSynchronizer抽象队列同步器）是**Java并发包java.util.concurrent的核心框架**，它是用来构建锁或者其他同步组件的基础框架，是JDK并发包中的重要基础设施。

AQS抽象了同步器的状态（state）以及等待队列（FIFO队列），它使用一个int成员变量来表示同步状态，通过内置的FIFO队列来完成资源获取线程的排队工作。并且，它提供了模板方法，供我们去实现我们的自定义同步器或者说自定义锁。

state访问方式：
   - getState()
   - setState()
   - compareAndSetState()

在AQS中，主要提供了两种对资源的共享方式：独占模式和共享模式。独占模式下，每次只能有一个线程持有锁，而共享模式下，多个线程可以同时持有锁。
   - Exclusive 独占资源-ReentrantLock
   Exclusive（独占，只有一个线程能执行，如 ReentrantLock）
   - Share 共享资源-Semaphore/CountDownLatch
   Share（共享，多个线程可同时执行，如 Semaphore/CountDownLatch/CyclicBarrier）。

总的来说，AQS的主要作用就是为我们提供了一种用于实现阻塞锁和相关同步器（信号量，事件，etc）的框架，大大简化了并发编程的难度。

### (41).了解Semaphore吗？ 

Semaphore（信号量）是一个计数信号量，主要用于管理一组资源，控制并发线程数。它可以用于做流量控制，特别公用资源有限的场合，如数据库连接。

Semaphore有两个主要的方法：acquire()和release()。
- acquire()方法用于申请资源，当Semaphore内部的计数器大于零时，线程调用acquire()会获取一个许可，计数器减1，线程继续执行。如果计数器为零，线程会被阻塞，直到有其他线程调用release()释放资源。
- release()方法用于释放资源，每调用一次，Semaphore内部的计数器就会加1，如果有线程因为调用acquire()方法而被阻塞，那么它将会被唤醒并得到许可。
它的主要作用就是控制同时访问特定资源的线程数量，例如实现一个文件允许的并发访问数量。

Semaphore可以有公平和非公平两种模式，公平模式下，调用acquire的线程如果无法获取许可，会被加入到等待队列中，以FIFO的方式获取许可；非公平模式下，线程可以尝试去无视等待队列去获取许可。

总的来说，Semaphore是一个非常实用的控制并发数的工具。
### (42).什么是Callable和Future?   

在Java中，Callable和Future通常用于表示和处理异步计算的结果。
   - Callable：Callable接口类似于Runnable，都是由它的实现类在另一个线程中执行的“任务”。但与Runnable不同的是，Callable有返回值，并且可以抛出异常。Callable的call()方法就是类似于Runnable的run()方法，只不过它可以返回一个值。
   - Future：Future是用来接收Callable任务执行后返回的结果，可以通过Future的get()方法来获取。get()方法会阻塞直到任务返回结果。Future提供了检查任务是否完成的方法，以等待计算的完成，并检索其结果。通过Future对象，我们可以取消任务，查询任务是否完成，获取任务结果等。

Callable和Future通常配合ExecutorService使用，提交Callable任务后，会返回一个Future对象，我们可以通过这个Future对象来获取Callable任务执行的结果。例如：
```java
ExecutorService executor = Executors.newFixedThreadPool(1);
Callable<Integer> task = () -> {
    try {
        TimeUnit.SECONDS.sleep(1);
        return 123;
    } catch (InterruptedException e) {
        throw new IllegalStateException("task interrupted", e);
    }
};
Future<Integer> future = executor.submit(task);
System.out.println("future done? " + future.isDone());
Integer result = future.get();
System.out.println("future done? " + future.isDone());
System.out.print("result: " + result);
```
总的来说，Callable和Future是Java中处理并发计算任务的重要工具，它们让我们可以更方便地处理异步任务的结果。
### (43).什么是阻塞队列？阻塞队列的实现原理是什么？如何使用阻塞队列来实现生产者-消费者模型？  
### (44).什么是多线程中的上下文切换？
在多线程编程中，上下文切换是指操作系统在进行线程调度时，将当前线程的执行上下文（包括寄存器、程序计数器、栈指针等）保存到内存中，并加载下一个线程的执行上下文，使得下一个线程可以继续执行。这个过程涉及到CPU的状态切换和线程上下文的切换，称为上下文切换。

上下文切换的发生主要有以下几个原因：

1. **时间片耗尽：** 当一个线程的时间片用完了，操作系统会进行线程调度，切换到下一个线程执行，从而产生上下文切换。

2. **阻塞和唤醒：** 当一个线程需要等待某个事件发生时（如I/O操作、锁等待），它会被阻塞，等待事件发生后被唤醒，这时会发生上下文切换。

3. **抢占式调度：** 如果一个更高优先级的线程需要执行，并且当前线程无法主动释放CPU，操作系统会进行抢占式调度，将CPU让给更高优先级的线程，从而产生上下文切换。

上下文切换会消耗系统资源（如CPU时间和内存），并且会影响多线程程序的性能。因此，需要合理设计线程调度策略，尽量减少上下文切换的次数，提高系统的并发处理能力。

**from《javaguide》：**
多线程编程中⼀般线程的个数都⼤于 CPU 核⼼的个数，⽽⼀个 CPU 核⼼在任意时刻只能被⼀个线程使⽤，为了让这些线程都能得到有效执⾏，CPU 采取的策略是为每个线程分配时间⽚并轮转的形式。当⼀个线程的时间⽚⽤完的时候就会重新处于就绪状态让给其他线程使⽤，这个过程就属于⼀次上下⽂切换。

概括来说就是：当前任务在执⾏完 CPU 时间⽚切换到另⼀个任务之前会先保存⾃⼰的状态，以便下次再切换回这个任务时，可以再加载这个任务的状态。任务从保存到再加载的过程就是⼀次上下⽂切换。

上下⽂切换通常是计算密集型的。也就是说，它需要相当可观的处理器时间，在每秒⼏⼗上百次的切换中，每次切换都需要纳秒量级的时间。所以，上下⽂切换对系统来说意味着消耗⼤量的CPU 时间，事实上，可能是操作系统中时间消耗最⼤的操作。

Linux 相⽐与其他操作系统（包括其他类 Unix 系统）有很多的优点，其中有⼀项就是，其上下⽂切换和模式切换的时间消耗⾮常少
### (45).什么是Daemon线程？它有什么意义？ 

Daemon线程（守护线程）是一种特殊类型的线程，它在后台运行，提供服务或支持其他非守护线程的工作。与普通线程（用户线程）不同的是，当所有非守护线程结束时，守护线程也会随之自动结束，即它们的生命周期依赖于非守护线程的存在。

守护线程通常用于在程序运行时执行一些后台任务，例如垃圾回收、内存管理、日志记录等，它们不需要在程序执行结束后继续运行。守护线程通常用来提供服务或支持非守护线程的工作，而不会影响程序的主要功能。使用守护线程可以减少资源消耗和提高程序的性能。

守护线程的创建方式与普通线程类似，通过Thread类的setDaemon()方法设置线程为守护线程。在Java中，主线程默认是非守护线程，而通过Thread类创建的线程默认也是非守护线程，只有通过setDaemon()方法设置为守护线程后才会成为守护线程。
### (46).乐观锁和悲观锁的理解及如何实现，有哪些实现方式？  

乐观锁和悲观锁主要是对数据并发操作的两种策略。
- 悲观锁：顾名思义，这种策略就是对数据被外界（包括本系统其他事务，以及来自外系统的事务处理）修改持保守态度，即假定会发生并发冲突，屏蔽一切可能违反数据完整性的操作。在关系数据库中，常见的行锁、表锁、排他锁等都是悲观锁的体现。悲观锁主要是在数据处理前加锁，确保在锁定期间内数据不会被其他线程修改。
- 乐观锁：相对于悲观锁来说，乐观锁则认为数据并发修改的可能性比较小，因此尽可能地减少加锁操作。乐观锁主要在数据提交更新时才进行判断是否存在冲突，如果存在冲突则进行相应的处理，通常是抛出异常或者进行重试。乐观锁的实现通常是基于数据版本（Version）记录或者CAS操作来实现。

具体的实现方式：
- 悲观锁：在Java中，常见的悲观锁实现方式就是synchronized关键字和Lock接口及其实现类，如ReentrantLock。在数据库中，可以通过SELECT ... FOR UPDATE语句来实现行级的悲观锁。
- 乐观锁：在Java的并发编程中，常用的乐观锁的实现方式就是Atomic包下的原子类，如AtomicInteger。在数据库中，乐观锁通常是通过版本控制实现的，每次更新数据时，会检查版本是否发生变化，如果版本发生变化，说明数据在此期间被其他线程修改过，那么就拒绝本次操作。

### (47).介绍一下automic原子类   
在Java中，Atomic（原子）类提供了一种在多线程环境下进行无锁（lock-free）线程安全的编程方式。Atomic类底层使用了CAS（Compare and Swap，即比较并交换）操作，CAS操作是一种无锁的技术，可以在多处理器上实现同步。

Java的Atomic包中有许多类，如AtomicInteger，AtomicLong，AtomicReference等。这些类为我们提供了丰富的原子操作，例如：
1. AtomicInteger：原子更新整型。提供了对int类型的封装，并提供了常用的数学运算方法，如incrementAndGet()（自增并返回），decrementAndGet()（自减并返回）等，它们都是原子操作。
2. AtomicLong：原子更新长整型。类似于AtomicInteger，但是它是操作long类型的数据。
3. AtomicBoolean：原子更新布尔类型。提供了对boolean类型的原子操作。
4. AtomicReference：原子更新引用类型。提供了对引用类型的原子操作。
5. AtomicIntegerArray，AtomicLongArray，AtomicReferenceArray：分别提供了对整型数组，长整型数组，引用类型数组的原子操作。
6. AtomicStampedReference：原子更新带有版本号的引用类型。可以防止ABA问题。
这些Atomic类的主要特点是提供了一种无锁的线程安全方式，相比于synchronized和Lock，它们更轻量级，性能通常更好。使用Atomic类能使我们的代码在多线程环境下更加安全，更加易于理解和维护。

from《JavaGuide》：

基本类型：
   - AtomicInteger ：整形原⼦类
   - AtomicLong ：⻓整型原⼦类
   - AtomicBoolean ：布尔型原⼦类

数组类型：
   - AtomicIntegerArray ：整形数组原⼦类
   - AtomicLongArray ：⻓整形数组原⼦类
   - AtomicReferenceArray ：引⽤类型数组原⼦类

引用类型：
   - AtomicReference ：引⽤类型原⼦类
   - AtomicStampedReference ：原⼦更新带有版本号的引⽤类型。该类将整数值与引⽤关联起来，可⽤于解决原⼦的更新数据和数据的版本号，可以解决使⽤ CAS 进⾏原⼦更新时可能出现的 ABA 问题。
   - AtomicMarkableReference ：原⼦更新带有标记位的引⽤类型
 
对象的属性修改类型：
   - AtomicIntegerFieldUpdater ：原⼦更新整形字段的更新器
   - AtomicLongFieldUpdater ：原⼦更新⻓整形字段的更新器
   - AtomicReferenceFieldUpdater ：原⼦更新引⽤类型字段的更新器
### (48).多线程中的问题（内存泄漏、上下文切换、死锁）