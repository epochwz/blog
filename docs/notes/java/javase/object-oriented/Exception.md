# 异常处理

**定义**：程序运行过程中意外发生的、背离程序本身意图的状况，都称之为**异常 (Exception)**

## 异常体系概述

**Java 异常的分类**

```bash
Throwable
    Error       程序本身无法处理的错误。
                这些错误通常是 JVM 产生的，表示在运行应用程序的过程中发生了严重的、不可修复的错误。
                这些错误通常在程序的控制和处理能力之外，因此程序不应该试图去处理这类错误。
                举例：VirtualMachineError、OutOfMemoryError、StackOverflowError、ThreadDeath
    Exception   程序本身可以处理的错误。
                这些错误是程序应该避免或者主动进行处理的。
                我们平时所说的异常处理就是针对这种类型的异常进行处理的。
        UnCheckedException  运行期间才会产生的、编译器不强制要求处理的异常，包括 RuntimeException 及其子类
            举例：NullPointerException、ArrayIndexOutOfBoundsException、ClassCastException
        CheckedException    编译阶段就可以发现的、编译器强制要求处理的异常，除了 RuntimeException 及其子类以外的其他 Exception 子类
            举例：IOException、SQLException
```

## 异常处理机制

**定义**：Java 运行时环境对于程序产生的异常的一整套处理流程，称为**异常处理机制**

**作用**：实现 "业务功能代码" 和 "错误处理代码" 分离，提高程序的可读性、容错性和健壮性。

**规定**：必须 声明抛出 / 捕获 `CheckedException`, 允许选择性的处理或者忽略 `UnCheckedException & Error`

**异常处理流程**

1. 当程序中出现错误时，会产生一个异常对象（包含 异常类型、异常出现时的程序状态 等信息），并交由运行时环境处理
2. 运行时环境会逐级（从产生异常的最底层调用者到最上层调用者）抛出该异常对象，直到该异常对象被显式捕获
3. 如果该异常对象被捕获 (`try`)，则运行时系统就会逐步寻找该异常对应的处理器（`catch`）并执行相关的处理逻辑
   - “合适”的含义：该异常对象 是 catch 块声明的异常类型或其子类的实例
4. 如果未找到合适的处理器，则继续将异常抛出给上一级调用者，回到步骤 2
5. 如果该异常对象始终没有被捕获，则最终由 JVM 处理该异常：打印异常信息、退出运行时环境并终止程序

## 异常处理方式

**三种异常处理方式**

| 处理方式              | 语法                    | 备注                             |
|:---------------------|:------------------------|:---------------------------------|
| 捕获并处理异常        | `try...catch...finally` |                                  |
| 声明方法可能抛出的异常 | `throws`                | 交由上一级调用者处理              |
| 主动抛出异常          | `throw`                 | 只能抛出 Throwable 及其子类的实例 |

**语法规则**

- `try 块` 后面必须至少跟随 1 个 `catch 块` 或 `finally 块`
- `catch 块` 和 `finally 块` 无法脱离 `try 块` 单独使用
- `catch 块` 和 `finally 块` 可以同时使用
- 先处理小异常，再处理大异常：父类异常 catch 块 必须放在 子类异常 catch 块 之后，否则编译出错
- 综合代码示例

    ```java
    // 5. throws 用于在方法签名中声明该方法可能向上一级调用者抛出的异常类型
    public void exceptionTest throws Exception(){
        try(
            // 7. 声明 / 初始化 程序结束时需要显式关闭的资源『数据库连接、网络连接』 变量
            //    try 块会在该语句结束时自动关闭这些资源，相当于包含了隐式 finally 块
        ){
            // 1. try 块是可能引发异常的代码块
        }catch(A e){
            // 2. catch 块负责捕获并处理 try 块异常

            // 6. 单异常捕获：异常变量不会被 final 修饰，可以重新赋值
            e = new Exception("test");
        }catch (B | C ie){
            // 6. 多异常捕获：异常变量默认被 final 修饰，因此无法重新赋值
            // ie = new ArithmeticException("test"); // 编译出错
        }finally{
            // 3. finally 块用于显式回收物理资源『垃圾回收机制只能回收内存资源』
            //    异常机制保证该代码块总会被执行，除非直接调用退出虚拟机的方法 System.exit(0)

            // 4. throw 用于向上一级调用者抛出一个具体的异常对象
            throw new IOException();
        }
    }
    ```

- 通常不在 finally 块中使用 `return/throw` 等终止方法的语句，否则会导致 try/catch 中的 return 语句失效

    ```java
    // 返回 false
    public static boolean test(){
        try{
            return true;
        }finally{
            return false;
        }
    }
    // 当程序执行 try...catch 块 时遇到 return/throw 语句，并不会立即结束该方法，而是去寻找 finally 块
    // 若没有 finally 块，则立即执行 return/throw 语句，方法终止
    // 若有 finally 块，则执行 finally 块，然后再次跳回 try...catch 块中执行 return/throw 语句
    // 若 finally 中包含 return 语句，则导致方法提前终止，无法跳回 try...catch 块执行其 return/throw 语句
    ```

- `throw` 抛出异常：抛出的不是异常类，而是异常实例，且每次只能抛出一个异常实例

    ```java
    // Java 7 之前，编译器认为①号代码抛出的异常类型就是捕获时的类型，所以此处必须声明抛出 Exception
    // public static void throwActually() throws Exception{
    // Java 7 开始，JVM 会检查①号代码抛出的异常的实际类型，所以此处只需声明抛出 FileNotFoundException 即可
    public static void throwActually() throws FileNotFoundException{
        try{
            new FileInputStream("a.txt");
        }catch (Exception ex){
            ex.printStackTrace();
            // 使用 throw 关键字将异常抛出，留待上一级调用者处理
            throw ex;        // ①
        }
    }
    ```

## 异常处理原则

**异常处理的原则**：隐藏原始异常信息，向上提供必要的异常提示信息，保证底层异常不会扩散到表现层，避免向上层暴露太多实现细节。

- 至少打印异常信息
- 尽量处理 `CheckedException`
- 当前层明确知道如何处理异常：捕获并处理
- 当前层只能处理部分异常：捕获并处理，然后将异常包装成当前层的异常，重新抛出给上一级调用者
- 当前层完全不知道如何处理异常：不要捕获，直接声明抛出该异常，交由上一级调用者进行处理

## 自定义异常类

**自定义异常类**：继承 `Exception` / `RuntimeException` 基类，提供 无参构造器、带一个字符串参数的构造器『该字符串作为异常对象的描述信息，即 `e.getMessage()` 的返回值』

## 其他问题

如何解释下面的代码输出结果？

```java
class Test {
    public static void main(String[] args) {
        System.out.println(test(10)); // 20
    }

    public static int test(int num) {
        try {
            num += 10;
            return num;
        } finally {
            num += 10; // 此处会被执行，因此 num=30，但 main 方法中输出仍为 20，应该如何解释？
        }
    }
}
```

**反编译**上面的代码，得到代码如下，可以解释上面代码的输出结果

```java
class Test {
    public Test() { }

    public static void main(String[] args) {
        System.out.println(test(10));
    }

    public static int test(int num) {
        int var1;
        try {
            num += 10;
            var1 = num;
        } finally {
            num += 10;
        }
        return var1;
    }
}
```
