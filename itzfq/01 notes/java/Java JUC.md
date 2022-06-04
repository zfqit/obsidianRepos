# Java JUC

## 概念

> 什么是JUC?

​	JUC是java.util.concurrent包的简称，在Java5.0添加，目的就是为了更好的支持高并发任务。让开发者进行多线程编程时减少竞争条件和死锁的问题！

> 进程与线程的区别

```markdown
进程: 一个运行中的程序的集合; 一个进程往往可以包含多个线程,至少包含一个线程

java默认有几个线程? 两个 main线程 gc线程

线程 : 线程（thread）是操作系统能够进行运算调度的最小单位。
```

> 并发与并行的区别

```markdown
并发(多线程操作同一个资源,交替执行)
CPU一核, 模拟出来多条线程,天下武功,唯快不破,快速交替
并行(多个人一起行走, 同时进行)
CPU多核,多个线程同时进行 ; 使用线程池操作
```

> 线程有六个状态

```java
public enum State {
    // 新生
    NEW,

    // 运行
    RUNNABLE,

    // 阻塞
    BLOCKED,

    // 等待
    WAITING,

    //超时等待
    TIMED_WAITING,

    //终止
    TERMINATED;
}
```

## Lock锁

> lock锁是什么?

```markdown
锁是用于通过多个线程控制对共享资源的访问的工具。通常，锁提供对共享资源的独占访问：一次只能有一个线程可以获取锁，并且对共享资源的所有访问都要求首先获取锁。 但是，一些锁可能允许并发访问共享资源，如ReadWriteLock的读写锁。 

在Lock接口出现之前，Java程序是靠synchronized关键字实现锁功能的。JDK1.5之后并发包中新增了Lock接口以及相关实现类来实现锁功能。

虽然synchronized方法和语句的范围机制使得使用监视器锁更容易编程，并且有助于避免涉及锁的许多常见编程错误，但是有时您需要以更灵活的方式处理锁。例如，用于遍历并发访问的数据结构的一些算法需要使用“手动”或“链锁定”：您获取节点A的锁定，然后获取节点B，然后释放A并获取C，然后释放B并获得D等。在这种场景中synchronized关键字就不那么容易实现了，使用Lock接口容易很多。

**Lock接口的实现类：**
可重入锁(常用) 			写锁									读锁
ReentrantLock, ReentrantReadWriteLock.ReadLock, ReentrantReadWriteLock.WriteLock 
```

> Synchronized 和 Lock 的主要区别

- **存在层面**：Syncronized 是Java 中的一个关键字，存在于 JVM 层面，Lock 是 Java 中的一个接口
- **锁的释放条件**：1. 获取锁的线程执行完同步代码后，自动释放；2. 线程发生异常时，JVM会让线程释放锁；Lock 必须在 finally 关键字中释放锁，不然容易造成线程死锁
- **锁的获取**: 在 Syncronized 中，假设线程 A 获得锁，B 线程等待。如果 A 发生阻塞，那么 B 会一直等待。在 Lock 中，会分情况而定，Lock 中有尝试获取锁的方法，如果尝试获取到锁，则不用一直等待
- **锁的状态**：Synchronized 无法判断锁的状态，Lock 则可以判断
- **锁的类型**：Synchronized 是可重入，不可中断，非公平锁；Lock 锁则是 可重入，可判断，可公平锁
- **锁的性能**：Synchronized 适用于少量同步的情况下，性能开销比较大。Lock 锁适用于大量同步阶段：

- **存在层面**：Syncronized 是Java 中的一个关键字，存在于 JVM 层面，Lock 是 Java 中的一个接口
- **锁的释放条件**：1. 获取锁的线程执行完同步代码后，自动释放；2. 线程发生异常时，JVM会让线程释放锁；Lock 必须在 finally 关键字中释放锁，不然容易造成线程死锁
- **锁的获取**: 在 Syncronized 中，假设线程 A 获得锁，B 线程等待。如果 A 发生阻塞，那么 B 会一直等待。在 Lock 中，会分情况而定，Lock 中有尝试获取锁的方法，如果尝试获取到锁，则不用一直等待
- **锁的状态**：Synchronized 无法判断锁的状态，Lock 则可以判断
- **锁的类型**：Synchronized 是可重入，不可中断，非公平锁；Lock 锁则是 可重入，可判断，可公平锁
- **锁的性能**：Synchronized 适用于少量同步的情况下，性能开销比较大。Lock 锁适用于大量同步阶段：

