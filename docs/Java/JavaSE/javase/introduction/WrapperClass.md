# 包装类

**定义**：每种基本数据类型相对应的引用数据类型，称为**包装类 (Wrapper Class)**

**作用**：解决了基本数据类型不能当成 Object 类型变量使用的问题（没有属性、方法，无法进行对象化交互）

## 基本数据类型和包装类的类型转换

**装箱和拆箱**：基本数据类型和包装类之间的类型转换

- **自动装箱**：允许把一个 `基本类型变量 / 字面值` 直接赋给对应的 `包装类变量`
- **自动拆箱**：允许把一个 `包装类对象` 直接赋值给对应的 `基本类型变量`
- **手动装箱**：显式的使用包装类的方法 把一个 `基本类型变量 / 字面值` 直接赋给对应的 `包装类变量`
- **手动拆箱**：显式的使用包装类的方法 把一个 `包装类对象` 直接赋值给对应的 `基本类型变量`
- 代码示例

    ```java
    int t1=2;
    Integer i1 = t1; // 自动装箱
    Integer i3 = Integer.valueOf(2); // 手动装箱

    Integer i2 = new Integer(2); // 手动装箱

    int t2 = i1; // 自动拆箱
    int t3 = i1.intValue(); // 手动拆箱
    ```

## 基本数据类型和字符串的类型转换

### 基本数据类型转字符串

```java
int i=999;
// 1. 使用 包装类 的方法
String s1 = Integer.toString(i);
// 2. 使用 字符串 的方法
String s3 = String.valueOf(i);
// 3. 使用 连接符 的方法：任意基本数据类型与字符串连接，都会自动转成字符串
String s2 = "" + i;
```

### 字符串转基本数据类型

```java
String s="999"
// 1. 直接转换
int i1 = Integer.parseInt(s);
// 2. 先将字符串转成包装类，再进行自动拆箱
int i2 = Integer.valueOf(s);
// 3. 先将字符串转成包装类，再进行自动拆箱
int i3 = new Integer(s);
```

## 基本数据类型和包装类的数值比较

包装类实例与基本类型变量 / 字面值比较时，将自动拆箱后进行数值比较

```java
// 包装类变量可以直接与基本数据类型的值进行比较『自动拆箱』
System.out.println("包装类与基本数值类型值比较：" + (new Integer(8) > 5)); // true
// 包装类变量可以直接与基本数据类型的值进行比较『自动拆箱』
System.out.println("包装类与基本数值类型值比较：" + (new Integer(5) == 5)); // true

// 两个包装类实例之间进行 [>、<、>=、<=] 的比较时，也会进行数值比较『自动拆箱』
System.out.println("包装类之间数值的比较：" + (new Integer(6) > new Integer(5))); // true
// 两个包装类实例之间进行 [==、!=] 的比较时，则是引用类型之间的比较
System.out.println("包装类之间引用的比较：" + (new Integer(5) != new Integer(5))); // true
```

## 不可变性

包装类和字符串一样具有不可变性，一旦创建之后就无法重新赋值；如果重新赋值，则实际上是重新创建了一个对象，然后将变量指向新的对象

```java
Integer x = 126;
Integer y = x;
System.out.println(x == y); // true
x++; // 包装类和字符串一样具有不可变性，因此此处会创建一个新的 Integer 对象 127，然后将 x 指向这个新的对象
System.out.println(x == y); // false
System.out.println(x);  // 127
System.out.println(y);  // 126
```

```java
Integer x = new Integer(128);
Integer y = x;
System.out.println(x == y); // true
x = 129; // 包装类和字符串一样具有不可变性，因此此处会创建一个新的 Integer 对象 129，然后将 x 指向这个新的对象
System.out.println(x == y); // false
System.out.println(x); // 129
System.out.println(y); // 128
```

## 缓存池

包装类中除了 `Float` 和 `Double` 之外，都持有一个缓存池，用于缓存特定值的包装类实例

- 使用 构造器 `new WrapperClass()` 创建包装类实例时，一定是重新开辟内存空间，创建新的实例

    ```java
    Integer i3 = new Integer(2);
    Integer i4 = new Integer(2);
    System.out.println("使用 构造器 创建的 [-128,127] 范围内的实例是否相等：" + (i3 == i4)); // false
    ```

- 使用 `自动装箱` 或者 `WrapperClass.valueOf()` 创建 `[-128,127]` 范围内的包装类实例时，将使用缓存池中的实例

    ```java
    // [-128,127] 范围内 自动装箱 / valueOf 将使用缓存池里的实例
    Integer i1 = 2;
    Integer i2 = 2;
    Integer i9 = Integer.valueOf(2);
    System.out.println("使用 自动装箱 创建的 [-128,127] 范围内的实例是否相等：" + (i1 == i2)); // true
    System.out.println("使用 自动装箱 和 valueOf 创建的 [-128,127] 范围内的实例是否相等：" + (i1 == i9)); // true

    // [-128,127] 范围外 自动装箱 无法使用缓存池里的实例
    Integer i5 = 128;
    Integer i6 = 128;
    Integer i10 = Integer.valueOf(128);
    System.out.println("使用 自动装箱 创建的 [-128,127] 范围外的实例是否相等：" + (i5 == i6)); // false
    System.out.println("使用 自动装箱 和 valueOf 创建的 [-128,127] 范围外的实例是否相等：" + (i5 == i10)); // false
    ```
