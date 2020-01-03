# 变量和常量

## 变量

### 变量的定义

Java 程序的数据存储在内存中，而我们需要一个“名字”来代表某块内存单元，从而可以访问这个内存单元的数据，这个“名字”就是**变量 (Variable)**

*变量代表一块 计算机内存单元，是程序用于访问 / 修改这块内存单元所存储的数据的途径*

### 变量三要素

1. 变量数据类型

    ```bash
    基本数据类型：基础的数值类型，定义的变量直接存储数据内容
        整数型
            byte    1 字节
            short   2 字节
            int     4 字节
            long    8 字节
        浮点型
            float   4 字节
            double  8 字节
        字符型
            char    2 字节
        布尔型
            boolean 1 字节
    引用数据类型：复杂的数据类型，定义的变量不直接存储实际的数据内容，而是存储一个内存地址，该内存地址指向堆内存中实际的数据内容（对象）
        类
        接口
        数组
    ```

2. 变量名：源程序中的 变量的名字
3. 变量值：源程序中的 字面值 或 其他变量
   - 字面值：源程序中直接给出的值，如数值、字符、字符串、布尔值、true/false/null 等

### 变量的使用

- 变量声明 `数据类型 变量名;`
- 变量赋值 `变量名 = 变量值;`
- 变量初始化（声明并赋值） `数据类型 变量名 = 变量值;`

### 变量的默认值

| 数据类型 | 关键字                   | 默认值   |
|:--------|:------------------------|:---------|
| 整数型  | `byte、short、int、long` | `0`      |
| 浮点型  | `float、double`         | `0.0`    |
| 字符型  | `char`                  | `\u0000` |
| 布尔型  | `boolean`               | `false`  |
| 引用类型 | `类、接口、数组、字符串`  | `null`   |

## 常量

### 常量的定义

Java 程序中使用 `final` 修饰的 *变量* 被称作 **常量 (Constant)**

### 常量的性质

常量初始化（赋值）完成之后就永远无法再次改变（赋值）了

- **基本类型常量**：基本类型常量保存的就是变量的实际内容，由于无法对常量重新赋值，所以基本类型常量的内容不可改变
- **引用类型常量**：引用类型常量保存的只是对象的内存地址，虽然无法对常量重新赋值，导致引用类型常量指向的内存地址不可改变，但其内存地址所指向的对象的内容仍然可以被修改

### 常量的使用

- 常量声明 `final 数据类型 常量名;`
- 常量赋值 `常量名 = 常量值;`
- 常量初始化（声明并赋值） `final 数据类型 常量名 = 常量值;`

### 宏变量

**宏变量**：声明时就指定了初始值，且指定的初始值在编译期间就能够确定的 *常量*，称作 “宏变量”

**宏替换**：宏变量相当于一个字面值，编译器会把程序中所有用到宏变量的地方直接替换成这个字面值

**宏替换代码示例**

- 示例：编译期间发生的 “宏替换”

  ```java
  class MacroReplacement{
    public static void main(String[] args) {
        // 下面定义了 2 个“宏变量”
        final int a = 5 + 2;
        final double b = 1.2 / 3;
        // 因此如下两句代码是等价的，系统会自动将使用到 “宏变量” a 和 b 的地方替换成该 “宏变量” 的值
        System.out.println(a + "\t" + b);
        System.out.println(7 + "\t" + 1.2 / 3);

        // 这段代码编译后如下：“宏变量” 将直接被替换成字面值
        
        // int a = true;
        // double b = 0.39999999999999997D;
        // System.out.println("7\t0.39999999999999997");
        // System.out.println("7\t0.39999999999999997");
    }
  }
  ```

- 示例：典型的 “宏替换” 细节分析

    ```java
    class MacroVariableTest {
        public static void main(String[] args) {
            macroVariable1();
            macroVariable2();
        }
    
        public static void macroVariable1() {
            // str0 的值在编译期可以确定，因此 str0 指向常量池中的 "HelloWorld"
            // 因为 str1 和 str2 是 “宏变量”，所以编译期间会执行 “宏替换”，使用到它们的地方都会被替换成字面值
            // 因此 str3 的值在编译期可以确定，所以 str3 指向常量池中的 "HelloWorld"
            String str0 = "Hello" + "World";
            final String str1 = "Hello";
            final String str2 = "World";
            String str3 = str1 + str2;
            System.out.println(str3 == str0); // true
        }
    
        public static void macroVariable2() {
            // str0 的值在编译期可以确定，因此 str0 指向常量池中的 "HelloWorld"
            // 因为 str1 和 str2 不是 “宏变量”，不会执行 “宏替换”
            // 因此 str3 的值在编译期无法确定，所以 str3 指向内存中新创建的字符串对象
            String str0 = "Hello" + "World";
            String str1 = "Hello";
            String str2 = "World";
            String str3 = str1 + str2;
            System.out.println(str3 == str0); // false
        }
    }
    ```