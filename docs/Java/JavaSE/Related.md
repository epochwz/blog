# 相关知识

Java 源文件最多只能有一个 `public` 类，且该源文件的文件名必须与其一致

## Java 平台简介

| 缩写   | 英文                     | 中文            | 适用场景                                                                              |
| ------ | ------------------------ | --------------- | ------------------------------------------------------------------------------------- |
| JavaSE | Java Standard Edition    | Java 标准版     | 桌面程序                                                                              |
| JavaEE | Java Enterprise Edition  | Java 企业版     | Web 程序                                                                              |
| JavaME | Java Micro Edition       | Java 微型版     | 移动设备                                                                              |

## JDK、JRE、JVM

| 缩写   | 英文                     | 中文            | 备注                                                                              |
| ------ | ------------------------ | --------------- | ------------------------------------------------------------------------------------- |
| JVM    | Java Virtual Machine     | Java 虚拟机     | 抽象的计算机，负责执行指令，管理数据、内存和寄存器                                    |
| JRE    | Java Runtime Environment | Java 运行环境   | 运行 Java 程序的必须环境 (**JVM**、其他类加载器、字节码校验器、大量基础类库)          |
| JDK    | Java Development Kit     | Java 开发工具包 | 提供开发（编译、运行） Java 程序所需的各种工具和资源 (**JRE**、Java 编译器、常用类库) |

## Java 程序执行机制

### 高级语言运行机制

**编译型语言**[C、C++、Object-C、Pascal]

- 使用特定平台『操作系统』的编译器，将源程序全部**编译**成该平台的机器码，并包装成该平台所能识别的可执行性程序的格式
- 优点：编译好的程序可以脱离开发环境，直接在特定平台上独立运行，效率较高
- 缺点：编译好的程序只能在特定平台上运行，移植性差

**解释型语言**[Ruby、Python]

- 使用特定平台『操作系统』的解释器，将源程序逐行**解释**成该平台的机器码并立即执行
- 优点：只需提供特定平台的解释器即可方便的实现源程序的移植
- 缺点：每次都需要解释执行，运行效率通常较低，且不能脱离解释器独立运行

### Java 程序执行过程

*Java 是一种**半编译**、**半解释**的高级语言*

![java-program-execution-procedures][]

### Java 程序编译运行命令

| 功能 | 命令                         | 备注                                                                                     |
| ---- | ---------------------------- | ---------------------------------------------------------------------------------------- |
| 编译 | `javac -d desDir srcFile`    | `desDir` 是编译生成的字节码文件的存放路径；`srcFile` 是 Java 源文件所在的绝对 / 相对路径 |
| 运行 | `java package.javaClassName` | `java 完整包名 . 类名`                                                                   |

## Java 程序发布方式

- 用平台相关的编译器将整个应用编译成平台相关的可执行性文件『丧失了跨平台性』
- 为应用编写启动脚本，例如 `java package.MainCalss` 或 `start javaw package.MainCalss` 等
- 将应用程序制作成可执行压缩包

| 缩写 | 英文                    | 中文                                               |
| :--- | :---------------------- | :------------------------------------------------- |
| JAR  | Java Archive File       | Java 归档文件                                      |
| WAR  | Web Archibe File        | Web 归档文件                                       |
| EAR  | Enterprise Archive File | 企业应用归档文件『由 Web 应用和 EJB 两个部分组成』 |

## 环境变量

**变量**：计算机中某个内存单元的名称，操作系统/程序可以通过这个名称访问到该内存单元的数据

**环境变量**：在操作系统层面定义的变量

- **PATH**：该变量定义了操作系统搜索命令的路径

    ```bash
    PATH=${JAVA_HOME}/bin:$PATH
    ```

- **CLASSPATH**：该变量定义了 JRE 搜索 Java 命令的路径

    ```bash
    CLASSPATH=.;${JAVA_HOME}/lib
    ```

## 字符集

**ASCII**(American Standard Code for Information Interchange,美国标准信息交换代码)：一套基于拉丁字母的电脑编码系统，主要用于显示现代英语和其他西欧语言

- 标准 ASCII 码：使用 7 位二进制数的组合来表示 128 种字符：表示大小写字母、标点符号、美式英语中的控制字符
- 扩展 ASCII 码：使用 8 位二进制数的组合来表示 256 种字符：表示特殊符号、外来语言的字母

**Unicode**, 万国码，统一码，其目的是支持世界上所有的字符集

*Java 语言内置支持 Unicode 字符集*

[java-program-execution-procedures]:../../images/java/java-program-execution-procedures.png
