# 并发编程常见面试问题

## 线程和进程的区别

## 并发和并行的区别

## 多线程和高并发的联系

## 同步阻塞和异步非阻塞的关系

## 线程基础

### 创建线程

#### 线程有几种实现方式？

这个问题从不同的角度来看，可能会有不同的答案

1. 从创建的角度来说，实现一个线程，有很多种方式，典型的比如说通过 `继承 Thread 类` 或者 `实现 Runnable 接口` 或者使用 `线程池、定时器等工具类` 都可以创建线程
2. 从本质上来讲的话，实现一个线程，归根到底就只有两种方式，要么是 `继承 Thread 类`, 要么是 `实现 Runnable 接口`, 同时这也是 `Oracle` 官方文档的说法
3. 从源码层面看的话，实现一个线程，实际上只有一种方式，那就是 通过 `构造一个 Thread 类`, 然后再调用其 start 方法来请求 JVM 启动该线程 (也就是执行该线程的 run 方法)

   ```java
   /**
    * Causes this thread to begin execution; the Java Virtual Machine
    * calls the <code>run</code> method of this thread.
    */
   public synchronized void start() {}
   ```

   至于上述两种线程的实现方式，实际上是两种 `构造 Thread 类` 的方式，或者说是 `实现线程 run 方法` 的两种方式

   - 一种是通过继承 Thread 类来直接重写线程的整个 run 方法
   - 一种是通过实现 Runnable 接口并将其传入 Thread 类，然后由 Thread 类的 run 方法来间接的调用 Runnable 实现类的 run 方法

     ```java
     @Override
     public void run() {
         if (target != null) {
             target.run();
         }
     }
     ```

#### 两种实现方式在本质上有什么异同？

线程的这两种实现方式在本质上其实是一样的，都是通过 `构造一个 Thread 类`, 然后再调用其 start 方法来请求 JVM 启动该线程 (也就是执行该线程的 run 方法)

因此两者的区别仅仅在于其 run 方法中内容的来源不同

- 一种来源于 Thread 继承类的重写
- 一种来源于 Runnable 实现类的实现

#### 为什么实现 Runnable 接口的方式更好？

1. 从代码耦合上看：线程要执行的 `具体的任务 (run 方法中的内容)` 应该与 `创建和运行线程的机制 (也就是 Thread 类)` 解耦
2. 从资源消耗上看

   - 如果 继承 Thread 类，那么每次需要执行一个任务时，只能重新创建一个独立的线程，从而造成较大的开销 (创建、执行、销毁)
   - 如果 实现 Runnable 接口，那么执行不同任务时就可以利用线程池等技术重复利用已创建的线程，从而减少创建和销毁线程带来的资源损耗

3. 从可扩展性上看：由于 Java 仅支持单继承，所以继承 Thread 类后会导致该类无法再继承其他类，从而限制了代码的可扩展性

### 启动线程

#### 简述 start 方法的执行流程？

1. 检查线程状态：只有处于 `NEW` 状态下的线程才可以被启动 (已启动或已结束的线程都不能再次启动)，否则就直接抛出 `IllegalThreadStateException`
2. 加入到线程组
3. 真正启动线程：调用 `start0()` 方法，交由 JVM 启动该线程并执行其 `run` 方法

#### 重复调用一个线程的 start 方法会怎样？

会抛出一个 `IllegalThreadStateException` 异常。因为线程在启动时首先会检查线程的状态，如果该线程已经被启动过，则直接抛出异常，不再往下执行

#### 为什么不直接使用 run 方法来启动线程？

因为直接调用线程的 run 方法就相当于只是在当前线程中调用了一个实例的普通方法；只有通过调用线程的 start 方法，才能交由 JVM 去启动一个新线程来执行它的 run 方法

### 停止线程

#### 如何正确停止线程？

因为 Java 没有提供任何机制来保证安全的停止线程，而是提供了 **中断(Interruption)** 这种线程协作机制，

所以正确停止线程的基本原则是使用 interrupt 进行中断通知，而不是强制停止

由于线程的中断机制是一种协作机制：一个线程需要通过发送中断信号来 "通知" 另一个线程停止运行，而被通知中断的线程自身拥有是否响应中断的权力 (包括是否停止以及何时停止)

所以想要正确的停止线程，依赖于中断请求方、中断响应方、低层次方法 (可能被线程调用的方法) 都遵循一种良好的中断处理编码规范，否则中断机制也无法正常发挥作用

- 中断请求方：发出中断信号
- 中断响应方：在线程逻辑中的适当位置 (例如循环中) 检测线程中断状态，并对可能抛出中断异常的地方进行捕获和处理
- 低层次方法：优先抛出中断异常，或者在捕获中断异常后，恢复 (重新设置) 线程中断状态

#### 为什么 Java 要使用中断机制来停止线程？

因为线程生命周期结束的问题往往会使任务、服务以及程序的设计和实现过程变得十分复杂，而一个行为良好的软件和勉强运行的软件的主要区别就在于 `是否能够完善的处理各种任务失败、关闭、取消的情况`

通常我们很少会希望某个线程、任务、服务立即停止，因为这种立即停止会导致共享的数据结构处于不一致状态 (这也是 `stop / suspend / resume` 等相关方法被弃用的原因)

反之使用中断请求的方式，可以使得程序具备更好的健壮性：被通知中断的线程可以在线程结束之前从容的处理后事，甚至决定自己是否真的要终止，毕竟被中断的线程本身要比发出中断请求的线程更加清楚自身运行结束时应该如何进行清理工作

### 生命周期

#### 说一说线程的生命周期？

1. 先简述线程生命周期的状态
2. 再简述线程状态的转移路径
   1. `NEW` -> `RUNNABLE` -> `TERMINATED` 之间的状态转移是不可逆的
   2. `BLOCKED / WAITING / TIMED_WAITING` 期间发生了异常时也会终止
   3. 线程通过 `wait` 进入 `WAITING/TIMED_WAITING` 状态，被 `notify` 后会直接进入 `BLOCKED` 状态
   4. 线程可以在 `RUNNABLE` 状态和 `BLOCKED / WAITING / TIMED_WAITING` 状态之间来回转换
3. 最后再详述状态的转移条件

![ThreadLifeCycle](/docs/images/notes/java/ThreadLifeCycle.png)

### 常用方法

#### 比较 sleep & wait 的异同

**相同点**

1. sleep & wait 都可以使线程进入阻塞状态 (WAITING / TIMED_WAITING)
2. sleep & wait 都可以响应中断
3. sleep & wait 都不占用 CPU 资源

**不同点**

1. wait 是 Object 类的实例方法，sleep 是 Thread 类的静态方法
2. wait 方法必须在同步代码块 / 方法中执行，而 sleep 则不需要
3. wait 如果不指定超时时间则会陷入永久等待，只能等待唤醒或中断

   sleep 必须指定休眠时间，超出时间后会自动退出阻塞状态

4. 在同步代码块 / 方法中执行 wait 会释放相应对象的 monitor 锁，而执行 sleep 则不会

#### 为什么 wait/notify/notifyAll 需要在同步代码块中使用，而 sleep 不需要？

- wait/notify 这种设计主要是为了让线程通信变得可靠，防止发生线程陷入永久等待的情况

  wait/notify 这种线程协作机制的设计初衷是为了在线程调用 wait 之后可以让另一个线程调用 notify 将其唤醒，但是如果没有同步代码块保护的话，就有可能出现线程 A 还没执行 wait, 就切换到线程 B 并执行了 notify, 然后才切换回线程 A 执行了 wait, 这时线程 A 就失去了被唤醒的机会，从而陷入永久等待
- sleep 只是针对当前线程本身的操作，不涉及和其他线程的协作，因此无需同步保护

#### 为什么 wait/notify/notifyAll 定义在 Object 类中，而 sleep 定义在 Thread 类中？

- 基本概念：定义在 Object 类中相当于把锁绑定到对象上，定义在 Thread 类中相当于把锁绑定到线程上
- 因为 wait/notify/notifyAll 是一种锁级别的操作，而锁是绑定到对象上的，而不是绑定到线程上 (Java 对象的对象头中使用了几个位来保存锁的状态)
- 每一个线程可能需要同时持有多个锁，并相互配合使用，如果将其定义在 Thread 类中，就失去了这种灵活性

#### 既然 wait 是属于 Object 类的，那么如果调用 thread.wait 会怎样？

的确可以将 Thread 实例作为锁对象来调用 wait 方法，但 Thread 是个特殊的类，在线程执行结束时会自动执行 notify, 因此可能会对我们的代码逻辑造成干扰，所以不推荐使用

#### 调用 notifyAll 之后没有抢夺到锁的线程会处于什么状态？

- 被 notifyAll 唤醒却没有抢到锁的线程会进入 `BLOCKED` 状态

- `Oracle` 官方文档中对于 `BLOCKED` 状态的说明如下

  > Thread state for a thread blocked waiting for a monitor lock. A thread in the blocked state is waiting for a monitor lock to enter a synchronized block/method or reenter a synchronized block/method after calling Object.wait.

- 可见这也是线程进入 `BLOCKED` 状态的两种方式中的第二种：线程在调用 wait 之后重新尝试进入 synchronized 代码块 / 方法
