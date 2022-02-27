# JVM

## 谈谈对Java的理解

1. 平台无关性（跨平台）：一次编译，到处运行 (Compile Once,Run anywhere)
2. GC
3. 语言特性：泛型、反射、Lambda
4. 面向对象：封装、继承、多态
5. 类库：集合、IO、网络、并发
6. 异常处理

### 谈谈 Java 的平台无关性

**Java 是如何实现平台无关性的**

Java 使用 不同平台的编译器 `javac` 将 统一格式的 `.java` 源文件 编译成 统一格式的 `.class` 字节码文件，并由不同平台的 `JVM` 解释器 `java` 执行时解释成 不同平台的机器指令

- 字节码文件：所有 Java 源文件都会被编译成统一的 字节码文件
- Java 虚拟机：所有的 Java 虚拟机都可以执行字节码文件，并将其将其转换成特定平台的机器指令
- Java 语言规范：比如规定了基本数据类型的取值范围和行为，使之在不同平台上有一致的表现

**为什么 JVM 不直接将源码解析成机器码去执行**

1. 性能：如果不保存成字节码，则每次执行都需要做各种检查 -- “解释”
2. 增强兼容性和扩展性：定义了统一的字节码，则 JVM 可以运行 “不同” 的语言 -- 只要将其他语言也编译成字节码就行了，因此 JVM 甚至做到了 “语言无关性”

**扩展知识**

- `javac` 前端编译器 `源文件 -> 词法分析 -> 符号流 -> 语法分析 -> 语法树 -> 语义分析 -> 语法树 -> 中间代码`

- `javap` 反汇编器，可以查看 javac 生成的字节码，也就是 JVM 加载字节码文件后真正执行的 Java 虚指令

    ```class
    $ javap -c fun/epoch/mmall/vo/ByteCodeSample
    
    Compiled from "ByteCodeSample.java"
    public class fun.epoch.mmall.vo.ByteCodeSample {
      public fun.epoch.mmall.vo.ByteCodeSample();
        Code:
           0: aload_0                                                                       # 对 this 句柄进行操作
           1: invokespecial #1                  // Method java/lang/Object."<init>":()V     # 调用父类进行初始化操作，super()
           4: return                                                                        # 退出构造方法
    
      public static void main(java.lang.String[]);
        Code:
           0: iconst_1              # 将常量 "1" 放到栈顶
           1: istore_1              # 将栈顶值存储到第一个局部变量 "i"
           2: iconst_5              # 将常量 "5" 放到栈顶
           3: istore_2              # 将栈顶值存储到第二个局部变量 "j"
           4: iinc          1, 1    # iincreament: 将第一个变量 +1
           7: iinc          2, 1    # iincreament: 将第二个变量 +1
          10: getstatic     #2                  // Field java/lang/System.out:Ljava/io/PrintStream;     # 获取静态域对象，压入栈顶
          13: iload_1                                                                                   # 将第一个局部变量压入栈顶
          14: invokevirtual #3                  // Method java/io/PrintStream.println:(I)V              # 调用方法输出变量
          17: getstatic     #2                  // Field java/lang/System.out:Ljava/io/PrintStream;     # 获取静态域对象，压入栈顶
          20: iload_2                                                                                   # 将第二个局部变量压入栈顶
          21: invokevirtual #3                  // Method java/io/PrintStream.println:(I)V              # 调用方法输出变量
          24: return                # 退出方法
    }
    
    ```

### JVM 如何加载字节码文件

JVM 是一个内存中的虚拟机（抽象的仿真计算机），有自己完善的硬件架构，如处理器、堆栈、寄存器、指令系统；JVM 屏蔽了与具体平台的相关信息，使得 Java 程序只需要生成在 JVM 运行的目标代码（即字节码）就可以在不同的平台上运行

JVM Architecture

- Class Loader: 加载特定格式的 class 文件到内存中
- Execution Engine: 解析字节码并交由操作系统执行
- Native Interface: 本地方法接口，使得 Java 可以调用库文件
- Runtime Data Area: JVM 运行时开辟的内存空间 --> Java 内存空间结构模型

## 谈谈反射

Java 反射机制是在运行状态中，对于任意一个类，都可以知道这个类的所有属性和方法；对于任意一个对象，都可以调用其任意属性和方法；这种动态获取信息以及动态调用对象方法的功能称为 Java 语言的反射机制

## 谈谈 ClassLoader

ClassLoader 在 Java 中有着非常重要的作用，它主要工作在 Class 装载的加载阶段，其作用主要是从系统外部获得class 二进制数据流。

ClassLoader 是 Java 的核心组件，所有的 Class 都是由 ClassLoader 加载的。

ClassLoader 负责将 Class 文件里的二进制数据流装载进系统，然后交给JVM进行连接、初始化等操作

ClassLoader 种类

- BootStrapClassLoader: C++ 编写的，负责加载核心库，java.*
- ExtClassLoader: Java 编写的，加载扩展库 javax.*
- AppClassLoader: Java 编写的，加载程序所在目录
- 自定义 ClassLoader: 定制化加载

**双亲委派机制**

1. 避免加载重复的字节码文件

类的加载方式

- 隐式加载：new
- 显式加载：loadClass,forName

类的装载过程

1. 加载：通过 ClassLoader 加载 class 文件，生成 Class 对象
2. 链接
   1. 校验：检查加载的 class 的正确性和安全性
   2. 准备：为类变量分配存储空间并设置类变量初始值，类变量随类型信息存放在方法区中
   3. 解析：（可选）JVM 将常量池内的符号引用转换为直接引用
3. 初始化：执行类变量赋值和静态代码块

Class.forName & loadClass 的区别

- Class.forName 返回的 Class 是已经完成初始化的
- loadClass 返回的是尚未链接的

## Java 内存模型

**按线程划分**

- 线程私有：程序计数器（字节码指令 no OOM）、虚拟机栈（java 方法，SOF & OOM）、本地方法栈（native 方法，SOF && OOM）
- 线程共享：MetaSpace（类加载信息，OOM）、Java堆（常量池（字面量和符号引用量 OOM），堆（数组和类对象 OOM））

- 程序计数器 (Program Counter Register)
  - 当前线程所执行的字节码行号指示器
  - 改变计数器的值来选取下一条需要执行的字节码指令
  - 和线程一对一：多线程切换时需要恢复线程执行状态，因此每个线程都需要一个独立的程序计数器。
  - 对 Java 方法记录行号，对 native 方法则计数器值记录为Undefined
  - 不会发生内存泄露
- Java 虚拟机栈(Stack)
  - Java 执行方法的内存模型
  - 执行每个方法时都会为其创建一个方法栈帧及方法运行期间的基础数据结构，栈帧用于存储局部变量表、操作栈、动态连接、返回地址等每个方法执行期中对应虚拟机栈帧从入栈到出栈的过程，当方法调用结束时，方法的栈帧被销毁
  - 包含多个栈帧
  - Java 虚拟机栈 存储了单个线程每个方法执行时的栈帧，栈帧则存储了方法执行期间的相关信息
    - 局部变量表：包含方法执行过程中的所有变量：参数、this引用、基本数据类型局部变量
    - 操作数栈：在执行字节码指令时用到，类似原生CPU寄存器；大部分JVM字节码把时间花费在操作数栈的操作上：入栈、出栈、复制、交换、产生消费变量

**常见问题**

JVM 三大性能调优参数

- -Xms: 堆的初始值
- -Xmx: 堆的最大值
- -Xss: 每个线程虚拟机栈（堆栈）的大小

Java 内存模型中堆和栈的区别

- 管理方式：栈会自动释放，堆靠GC
- 空间大小：栈比堆小
- 碎片数量：栈产生的碎片远小于堆
- 分配方式：栈支持静态和动态分配，堆只支持动态分配
- 效率：栈的效率比堆高

内存分配策略

- 静态存储：编译时确定每个数据目标在运行时的存储空间需求
- 栈式存储：数据区需求在编译时未知，运行时模块入口前确定
- 堆式存储：编译时或运行时模块入口都无法确定空间需求，完全的动态分配

## 垃圾回收

### 垃圾标记算法

- 引用计数法
- 可达性分析

### 垃圾回收算法

#### 标记-清除算法(Mark and Sweep)

1. 标记：从根集合进行扫描，对存活的对象进行标记
2. 清除：对堆内存从头到尾进行线性遍历，回收不可达对象

缺点：碎片化，产生大量不连续的内存空间，导致后续分配大内存对象时频繁触发垃圾回收

#### 复制算法(Copying)

将内存空间分成对象区和空闲区，对象在对象区创建，垃圾回收时，存活的对象从对象区复制到空闲区，清除对象区中的所有对象

适用于对象存活率低的场景

顺序分配内存、简单高效，解决了碎片化的问题

#### 标记-整理算法(Compacting)

1. 标记：从根集合进行扫描，对存活的对象进行标记
2. 清除：移动所有存活的对象，且按照内存地址次序依次排列，然后将末端内存地址以后的内存全部回收

优点

- 避免了内存碎片化
- 不需要设置两块内存互换区
- 适用于存活率高的场景

#### 分代收集算法(Generational Collector)

其他垃圾回收算法的组合

按照对象的生命周期长短划分不同的区域，分别采用不同的垃圾回收算法

提高 JVM 垃圾回收效率

1. Minor GC: 新生代，复制算法
 - Eden Space:
 - Survivor Space:
 - `Eden:From:To == 8:1:1`
2. Full GC: 老年代的回收，往往也伴随着新生代垃圾回收的触发


对象如何晋升到老年代

- 经历一定次数(15)的Minor GC后依然存活的对象
- Survivor 区中放不下的对象
- 新生成的大对象()

常用 GC 调优参数

- `-XX:SurvivorRatio`: Eden & Survivor 的比例，默认8:1
- `-XX:NewRatio`: 老年代和年轻代内存大小的比例
- `-XX:MaxTenuringSizeThreshold`: 对象从年轻代晋升到老年代需要经历的GC次数的最大阈值

**老年代**：存放生命周期较长的对象，采用标记-整理/清理算法

Full GC 比 Minor GC 慢，但执行频率低

Full GC 触发条件

- 老年代空间不足
- 永久代空间不足(JDK6/7)
- CMS GC 时出现 promotion failed,concurrent mode failure
- Minor GC 晋升到老年代的平均大小大于老年代的剩余空间
- 程序中直接调用 System.gc()
- 使用RMI来进行RPC或管理的JDK应用，每小时执行1次 FULL GC
