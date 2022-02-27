# 面向对象

## 程序设计方法

**结构化程序设计**

- 面向功能的程序设计方法：按功能来分析系统需求；自顶向下、逐步求精、模块化
- 面向数据流的处理方式：每个功能模块负责 接收数据 -> 处理数据 -> 输出数据
- 以函数为中心：`eat(user,food);`

**面向对象程序设计**

- 面向对象的程序设计方法：使用类、对象、继承、封装、消息等基本概念进行程序设计
- 面向对象的处理方式：将客观世界中具有相似性质的事物抽象成一个类
  - 类定义 = 属性『状态数据』+ 方法『行为特征』
- 以类为中心：`user.eat(food);`

## 类和对象

**类和对象** 是面向对象程序设计的基础：类是对象的抽象化，对象是类的具体化

- **类 (Class)**：类是对客观世界中具有相似 / 相同性质的一类事物的抽象定义，规定了该类事物具有的状态数据和行为特征
- **对象 (Object)**：对象是这一类事物实际存在的一个个体，因此又称为类的 **实例 (Instance)**

## 类的成员

### 类的成员的分类

**按 组成结构 分类**

```java
1. 类成员：使用 static 修饰的成员，属于整个类
    类初始化块
    类成员变量
    类方法
    静态内部类（普通类 / 抽象类 / 枚举类 / 接口）
2. 实例成员：非 static 修饰的成员，属于单个实例
    实例初始化块
    实例成员变量
    实例方法
    非静态内部类（普通类 / 抽象类 / 枚举类 / 接口）
3. 构造器 / 构造方法：方法名和类名相同且没有返回值的特殊方法
4. 局部变量
    代码块局部变量：在代码块内定义的变量
    方法体局部变量：在方法体内定义的变量
    方法形参局部变量：在方法参数列表中定义的变量，也称为形参变量
```

**按 组成单位 分类**

```java
1. 初始化块：用于执行类 / 实例的显式初始化代码
2. 构造器：用于创建类的实例『执行实例的显式初始化代码，并返回实例的引用地址』
3. 变量
    成员变量：定义类 / 实例所持有的状态数据
    局部变量：定义各自作用域内的状态数据
4. 方法：定义类 / 实例的所拥有的行为特征
5. 内部类（普通类 / 抽象类 / 接口 / 枚举类）: 定义类所拥有的更加复杂的状态数据和功能实现
```

### 类的成员的调用

**类的成员在调用时都需要一个调用者**

- 类成员
  - 通过 **类名** 调用 `class.staticMember`
  - 通过 **实例** 调用 `instance.staticMember`
  - 在类成员中调用其他类成员时，会默认通过 `类名` 调用，因此可以省略调用者
- 实例成员
  - 通过 **实例** 调用 `instance.nonStaticMember`
  - 通过 **this** 调用 `this.nonStaticMember`

    **this** 代表当前实例（当前谁在调用就是谁），通常作为当前类中实例成员的默认调用者，使得同一个类中的实例成员可以直接相互调用

  - 在实例成员中调用其他实例成员时，会默认通过 `this` 调用，因此可以省略调用者
- 构造器
  - 通过 **new** 调用 `new Constructor()`

**类成员不能直接访问实例成员，只能通过创建实例来访问实例成员**：如果没有这种限制，则可能出现 `初始化类成员、调用类成员` 时 `实例成员却还未初始化完成` 的情况，从而导致系统异常