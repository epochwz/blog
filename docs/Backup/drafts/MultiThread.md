# 多线程

**进程**：指可执行程序并存放在计算机存储器的一个指令序列，它是一个动态执行的过程

**线程**：比进程小的运行单位，是轻量级的“进程”，是进程的子程序，一个进程包含多个线程

所有线程类都必须实现 Runnalbe 接口

## 线程的创建

- 继承 Thread 类，重写 run 方法
- 实现 Runnable 接口，重写 run 方法
- 实现 Callable 接口，重写 call 方法
  - call 方法作为线程的执行体，具有返回值，并且可以对异常进行声明和抛出，使用 start 方法启动线程
  - 示例

    ```java
    class MyThread implements Callable<String> {
        @Override
        public String call() {
            return "通过实现 Callable 接口创建线程";
        }

        public static void main(String[] args) {
            MyThread myThread = new MyThread();
            FutureTask<String> task = new FutureTask<>(myThread);
            new Thread(task).start();
            try {
                System.out.println(task.get());
            } catch (InterruptedException | ExecutionException e) {
                e.printStackTrace();
            }
        }
    }
    ```

线程只能启动一次，再次启动将引发 `IllegalThreadStateException`

使用 Runnable 的原因

- Java不支持多继承：如果已经继承了其他类，就无法继承 Thread 类了，但是可以实现多接口
- 不打算重写 Thread 类的其他方法，则没必要继承 Thread 类，而是实现 Runnable 更简单

实现 Runnable 的类的实例变量可以被多个线程共享

## 线程的状态和生命周期

线程的状态

- 新建(New)
- 就绪(Runnable)
- 运行(Running)
- 阻塞(Blocked)
- 终止(Dead)

线程的生命周期：线程的状态转换过程

## 线程的常用方法

sleep: 让正在执行的线程休眠（暂停执行）指定的毫秒数

join: 抢占资源，其他线程必须等待调用该方法的线程执行结束后才能执行

## 线程优先级

Java提供了10个优先级：1-10表示，超过范围则抛出异常

主线程默认优先级5

优先级常量

- MAX_PRIORITY=10
- NORM_PRIORITY=5
- MIN_PRIORITY=1

## 线程的同步

多线程运行的问题

- 各个线程是通过竞争CPU时间来获得运行机会的
- 各个线程什么时候得到CPU时间，占用多久，都是不可预测的
- 一个正在运行的线程什么时候被暂停是不可预测的

synchroized 用于同步代码块、方法

## 线程的通信

wait: 中断方法的执行，使线程等待（阻塞）
notify: 唤醒处于等待状态的某个线程，使其结束等待
notifyAll: 唤醒处于等待状态的所有线程，使其全部结束等待