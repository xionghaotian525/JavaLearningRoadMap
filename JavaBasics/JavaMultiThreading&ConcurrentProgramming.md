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

### 1. 线程基础
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
     - yield
     - join
     - priority
   - 等待/通知
     - wait
     - notify
     - notifyAll
   - 中断机制
     - interrupt方法
     - InterruptedException
### 2. 线程池
   - Executor 框架：提供线程池管理的高级接口和类（如 `ExecutorService`，ScheduledExecutorService）。
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
   - ThreadPoolExecutor：可高度定制的线程池实现。
### 3. 并发工具类
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
### 4. 并发工具
   - CountDownLatch：允许一个或多个线程等待一系列事件发生。
   - CyclicBarrier：使一定数量的线程到达一个同步点后再一起继续执行。
   - Semaphore：控制同时访问某个资源或操作的线程数量。
   - Exchanger：两个线程在同一点交换数据的同步点。
   - 其它：
     - Fork/Join 框架：用于大任务分解成小任务并行执行，然后合并结果的框架。
     - BlockingQueue：用于生产者消费者模式的阻塞队列接口。
### 5. 并发集合
   - Concurrent 包：提供线程安全的集合类，如 **`ConcurrentHashMap`**、`CopyOnWriteArrayList`。
   ![](/Res/images/concurrent包.png)

### 6. 并发编程最佳实践

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
任何时候只有一个线程可以获得锁，也就是说只有一个线程可以运行synchronized 中的代码
2. 使用notifyall,可以唤醒 所有处于wait状态的线程，使其重新进入锁的争夺队列中，而notify只能唤醒一个。
3. wait() 应配合while循环使用，不应使用if，务必在wait()调用前后都检查条件，如果不满足，必须调用notify()唤醒另外的线程来处理，自己继续wait()直至条件满足再往下执行。
4. notify() 是对notifyAll()的一个优化，但它有很精确的应用场景，并且要求正确使用。不然可能导致死锁。正确的场景应该是 WaitSet中等待的是相同的条件，唤醒任一个都能正确处理接下来的事项，如果唤醒的线程无法正确处理，务必确保继续notify()下一个线程，并且自身需要重新回到WaitSet中.

### (4).sleep()和wait()有什么区别  

1. 对于sleep()方法，我们首先要知道该方法是属于Thread类中的。而wait()方法，则是属于Object类中的。
2. sleep()方法导致了程序暂停执行指定的时间，让出cpu该其他线程，但是他的监控状态依然保持着，当指定的时间到了又会自动恢复运行状态。在调用sleep()方法的过程中，线程不会释放对象锁。
3. 当调用wait()方法的时候，线程会放弃对象锁，进入等待此对象的等待锁定池，只有针对此对象调用notify()方法后本线程才进入对象锁定池准备，获取对象锁进入运行状态。

### (5).volatile是什么？可以保证有序性吗？  
### (6).Thread类中的start()和run方法有什么区别？  
### (7).为什么wait、notify、notifyAll这些方法不在Thread类里面？  
### (8).为什么wait和notify方法要在同步块中调用？  
### (9).Java中interrupted和isInterrupted方法的区别  
### (10).Java中synchronized 和 ReentrantLock 有什么不同？  
### (11).有三个线程T1,T2,T3,如何保证顺序执行？  
### (12).SynchronizedMap和ConcurrentHashMap有什么区别？  
### (13).什么是线程安全  
### (14).Thread类中的yield方法有什么作用？  
### (15).Java线程池中submit() 和 execute()方法有什么区别？  
### (16).说一说自己对于 synchronized 关键字的了解（重要）  
### (17).说说自己是怎么使用 synchronized 关键字？  
### (18).什么是线程安全？Vector是一个线程安全类吗？  
### (19).volatile关键字的作用？（重要）  
### (20).常用的线程池有哪些？  
### (21).什么是线程池？简述一下你对线程池的理解  
### (22).Java程序是如何执行的  
### (23).锁的优化机制了解吗？  
### (24).说说进程和线程的区别？  
### (25).产生死锁的四个必要条件？  
### (26).如何避免死锁？  
### (27).线程池核心线程数怎么设置呢？  
### (28).Java线程池中队列常用类型有哪些？  
### (29).线程安全需要保证几个基本特征？  
### (30).说一下线程之间是如何通信的？  
### (31).CAS的原理呢？  
### (32).CAS有什么缺点吗？  
### (33).引用类型有哪些？有什么区别？  
### (34).说说ThreadLocal原理？  
### (35).线程池原理知道吗？以及核心参数  
### (36).线程池的拒绝策略有哪些？  
### (37).说说你对JMM内存模型的理解？为什么需要JMM？  
### (38).多线程有什么用？为什么要使用多线程  
### (39).说说CyclicBarrier和CountDownLatch的区别？  
### (40).什么是AQS？  
### (41).了解Semaphore吗？  
### (42).什么是Callable和Future?   
### (43).什么是阻塞队列？阻塞队列的实现原理是什么？如何使用阻塞队列来实现生产者-消费者模型？  
### (44).什么是多线程中的上下文切换？  
### (45).什么是Daemon线程？它有什么意义？  
### (46).乐观锁和悲观锁的理解及如何实现，有哪些实现方式？  
### (47).介绍一下automic原子类   
