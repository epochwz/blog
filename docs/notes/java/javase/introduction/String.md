# 字符串

## 不可变

**不可变性**：String 对象一旦被创建，就不能修改了

## 常量池

**常量池 (constant pool)**: 在编译期被确定，并被保存在已编译的 `*.class` 文件中的一些数据（类 / 接口中的常量，字符串字面值）

**字符串常量**

- 当程序第一次使用某字符串字面值时，JVM 会使用常量池缓存该字面值，称为 “字符串常量”
  - 这里的字面值包括可以在编译时就计算出来的表达式值 `"hel"+"lo"`
- 当程序再一次使用该字符串字面值时，JVM 会直接使用常量池中的相应字符串常量
- 每个字符串常量在常量池中只有一个，不会产生多个副本

**字符串对象**

- 通过 new 关键字创建的字符串对象，由于是在运行时创建的，所以将被保存在运行时内存区，而不会被放入常量池中
- 当程序中第一次出现 `new String("hello")` 时，JVM 会先在常量池中产生 "hello" 字符串常量，再调用 String 类的构造器创建一个新的 String 对象，保存在堆内存中。因此 `new String("hello")` 产生了一个字符串对象和一个字符串常量

    ```java
    class StringConstantTest {
        public static void main(String[] args) {
            // s1 直接引用常量池中的"HelloWorld"
            String s1 = "HelloWorld";
            String s2 = "Hello";
            String s3 = "World";
            // s4 直接引用常量池中的"HelloWorld"：因为后面的字符串值可以在编译时通过计算就确定下来
            String s4 = "Hello" + "World";
            // s5 直接引用常量池中的"HelloWorld"：因为后面的字符串值可以在编译时通过计算就确定下来
            String s5 = "He" + "llo" + "World";
            // s6 不能引用常量池中的字符串：因为后面的字符串值不能在编译时就确定下来
            String s6 = s2 + s3;
            // s7 引用堆内存中新创建的 String 对象：使用 new 调用构造器将会创建一个新的 String 对象
            String s7 = new String("HelloWorld");
            // s8 引用堆内存中新创建的 String 对象：使用 new 调用构造器将会创建一个新的 String 对象
            String s8 = new String("HelloWorld");

            System.out.println(s1 == s4); // 输出 true
            System.out.println(s1 == s5); // 输出 true
            System.out.println(s1 == "HelloWorld"); // 输出 true
            System.out.println(s1 == s6); // 输出 false
            System.out.println(s1 == s7); // 输出 false
            System.out.println(s8 == s7); // 输出 false
            System.out.println(s8.equals(s7)); // 输出 true
        }
    }
    ```

## String、StringBuilder、StringBuffer

- String 具备不可变性，而 StringBuilder/StringBuffer 不具备
- StringBuilder 是线程不安全的，而 StringBuffer 是线程安全的
- 性能比较（字符串频繁改变的情况下）：StringBuilder > StringBuffer > String
