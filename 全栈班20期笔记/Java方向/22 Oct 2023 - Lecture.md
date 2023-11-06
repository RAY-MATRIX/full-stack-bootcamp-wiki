# 22 Oct 2023 - Lecture
## 并发基础

- Java 并发概念
    Java并发编程是指在Java程序中处理多线程和多任务执行的编程方式。在并发编程中，多个线程可以同时执行不同的任务，或者多个线程可以并行执行相同的任务。Java提供了丰富的工具和库来支持并发编程，包括以下关键概念和组件：

    - 线程（Thread）： 线程是Java中的基本执行单元，它代表了一个独立的执行路径。多线程编程允许多个线程同时执行，从而提高程序的性能和响应能力。

    - 同步（Synchronization）： 同步是一种机制，用于控制多个线程对共享资源的访问，以避免数据竞争和确保数据一致性。Java提供了关键字synchronized、volatile和java.util.concurrent包中的同步工具，如锁和信号量。

    - 互斥（Mutual Exclusion）： 互斥是一种机制，确保同时只有一个线程可以访问共享资源。锁是实现互斥的主要工具，Java中有多种锁类型，包括内置锁（synchronized关键字）、重入锁（ReentrantLock）等。

    - 并发集合（Concurrent Collections）： Java提供了一系列的并发集合类，如ConcurrentHashMap和ConcurrentLinkedQueue，这些集合类在多线程环境下提供了高效的数据结构，用于存储和操作数据。

    - 线程池（Thread Pool）： 线程池是一组可重用的线程，它们可以执行多个任务，减少线程创建和销毁的开销，提高性能。Java中的Executor框架和ThreadPoolExecutor类用于管理线程池。

    - 并发工具（Concurrent Utilities）： Java提供了一系列的并发工具，如CountDownLatch、CyclicBarrier、Semaphore等，用于实现常见的并发模式和协调多个线程之间的操作。

    - 原子操作（Atomic Operations）： Java提供了原子操作类，如AtomicInteger和AtomicLong，用于执行单一操作而不受干扰的操作，以确保数据的原子性。

    - 并发问题（Concurrency Issues）： 在并发编程中，开发人员需要考虑一系列问题，如死锁、活锁、竞态条件、线程安全性等。了解并解决这些问题是并发编程的一部分。

- JMM Java Memory Model
    Java内存模型（JMM）通过一系列规则和约束来解决并发问题，以确保多线程程序可以在各种平台上以一致和可预测的方式运行。JMM使用各种机制来保证线程之间的可见性、原子性和有序性，以避免并发问题的发生。JMM解决并发问题的关键包括以下几个方面：

    - **可见性（Visibility）**： JMM使用volatile关键字来确保共享变量的可见性，保证一个线程对共享变量的修改对其他线程是可见的。当一个变量被声明为volatile时，JMM确保所有线程都能看到最新的值。

    -** 原子性（Atomicity）**： JMM通过synchronized关键字和java.util.concurrent包中的原子类来保证特定操作是原子的，即要么完全执行，要么不执行。这可以防止多个线程同时修改共享变量而导致数据不一致。

    - **有序性（Ordering）**： JMM定义了一组规则来确保在不同线程中执行的操作的顺序不会与源代码中的顺序发生颠倒。这包括禁止编译器和处理器对指令进行重排序以及保证volatile变量的有序性。

    - **线程间同步（Thread Synchronization）**： JMM使用synchronized关键字和内置锁来实现线程之间的同步，确保在多个线程之间对共享资源的访问是互斥的。它通过禁止线程同时访问同步块来避免数据竞争和不一致性。

- JMM如何解决这些问题
    - volatile
    - syncronize
    - final
    - 一个规则
        - happpens-before
            - 在一个线程内，在程序前面的操作先行发生于后面的操作。
            - 一个unlock操作先行发生于后面对同一个锁的lock操作。
            - 对一个volatile变量的写操作先行发生于后面对这个变量的读操作。