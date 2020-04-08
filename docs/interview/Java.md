# 面试问题

## 谈谈你对 Java 的理解？

- 平台无关性
- GC
- 语言特性 (泛型、反射、Lambda 表达式)
- 面向对象
- 类库
- 异常处理

### Java 如何实现平台无关性

如下图所示，Java 源文件首先会被 **编译** 成字节码文件，再由不同平台的 JVM 进行 **解释** 和执行 (JVM 在执行字节码时将其转换成具体平台上的机器指令)，因此 Java 语言在不同的平台上运行时无需重新编译，从而实现了 **跨平台**，即 `一次编译，到处运行 (compile once,run everywhere) `

![Java 如何实现平台无关性](/docs/images/notes/java/cross-platform.png)

为什么 JVM 不直接将源码解析成机器码去执行？

- 高效性：如果不先编译成字节码的话，则每次执行源码时都需要重新编译 (进行各种语法检查、语义检查)，且每次执行完毕后这些检查结果都无法被保存下来，从而影响程序运行效率
- 兼容性：实现语言无关性，支持其他语言所生成的字节码

### JVM 如何加载字节码文件

Java 虚拟机是一种抽象化的计算机，通过在实际的计算机上仿真模拟各种计算机功能来实现的。

JVM 有自己完整的硬件架构，如处理器、程序计数器、堆栈等，还具有自己的指令系统

JVM 屏蔽了与具体操作系统平台相关的信息，使得 Java 程序只需要生成在 JVM 上运行的目标代码 (字节码)，就可以在多种平台上不加修改的运行

- Class Loader: 将符合格式要求的 class 文件加载到内存中
- Execution Engine: 对字节码指令进行解析
- Native Interface: 封装不同开发语言的代码库供 Java 调用
- Runtime Data Area: 运行时数据区域，JVM 内存的空间结构模型

答：Class Loader 将符合规范的 class 文件加载到内存中，然后由 Execution Engine 负责解析并执行

## 谈谈反射

Java 反射机制是在运行状态中，对于任意一个类，都能够知道这个类的所有属性和方法；对于任意一个对象，都能够调用它的任意属性和方法；这种动态获取类信息以及动态调用对象方法的功能称为 Java 语言的反射机制

## 类从编译到执行的全过程

1. 编译器将 Robot.java 源文件编译成 Robot.class 字节码文件
2. ClassLoader 将字节码转换成 JVM 中的 Class<Robot> 对象
3. JVM 利用 Class<Robot> 对象实例化多个 Robot 对象

## 谈谈 ClassLoader

ClassLoader 在 Java 中有着非常重要的作用，它主要工作在 Class 装载的 加载阶段，其主要作用是从系统外部获得 Class 的二进制数据流。

它是 Java 的核心组件，所有的 Class 都是由 ClassLoader 进行加载的，ClassLoader 负责通过将 Class 文件里的二进制数据流装载进系统，然后交给 Java 虚拟机进行连接、初始化等操作


ClassLoader 的种类

- BootStrapClassLoader: C++ 编写的，用于加载核心库 java.*
- ExtClassLoader:Java 编写的，用于加载扩展库 javax.*
- AppClassLoader: 加载程序所在目录 classpath
- 自定义 ClassLoader: 定制化加载 (磁盘、网络等等)

自定义 ClassLoader

findClass

defineClass

## 为什么使用双亲委派模型

避免加载多份字节码

类的加载方式

- 隐式加载：new
- 显式加载：loadClass,forName

类的装载过程

1. 加载：通过 ClassLoader 加载 class 字节码文件，生成 Class 对象
2. 链接
   1. 验证：检查加载的 class 的正确性和安全性
   2. 准备：为类变量分配存储空间并设置类变量初始值
   3. 解析：(可选) 将常量池内的符号引用替换成直接引用
3. 初始化：执行类变量赋值和静态代码块


## JVM 内存结构

### 程序计数器 (Program Counter Register)

- 当前线程所执行的字节码行号指示器 (逻辑)
- 改变计数器的值来选取下一条需要执行的字节码指令
- 和线程是一对一的关系，即 “线程私有”
- 对 Java 方法计数，如果是 Native 方法则计数器的值是 undefined
- 不会发生内存泄漏

### Java 虚拟机栈 (VM Stack)

- Java 方法执行时的内存模型
- 包含多个栈帧 (局部变量表、操作数栈、动态链接、返回地址)，当方法调用结束时，对应的栈帧会被自动销毁
  - 局部变量表：方法执行过程中产生的所有变量
  - 操作数栈：执行字节码指令时用于入栈、出栈、复制、交换、产生消费变量


### 方法区 (Method Area)

方法区：用于存储 Class 的相关信息，是 JVM 的规范，而元空间 (MetaSpace,JDK1.8+) 和 永久代 (PermGen) 都是方法区的具体实现

- 永久代使用 JVM 内存，而元空间使用本地内存，避免了 `java.lang.OutOfMemoryError:PermGen space`
- 字符串常量池存在永久代中，容易出现性能问题和内存溢出
- 类和方法信息的大小难以确定，给永久代大小的指定带来困难
- 永久代会给 GC 带来不必要的复杂性
- 方便 HotSpot 与其他 JVM 如 Jrockit 的集成
