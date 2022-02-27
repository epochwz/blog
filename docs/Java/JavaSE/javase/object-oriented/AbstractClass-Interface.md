# 抽象类和接口

## 抽象类

**抽象类和抽象方法的定义**

```java
[public] abstract class AbstractClassName [extends SuperClass] [implements interface1,interface2 ...]{
    [public/protected] abstract void/DataType method(); // 抽象方法，可有可无
    // 其余类的成员与普通类一样
}
```

**抽象方法**：使用 `abstract` 修饰的方法，只有方法签名，没有方法实现，只能被子类重写

**抽象类**：使用 `abstract` 修饰的类

- 含有抽象方法的类只能被定义为抽象类，而抽象类可以没有抽象方法
- 抽象类只能被继承，不能被实例化，因此抽象类的构造器只能用于被子类实例复用，而不能用于创建实例

**一个类拥有抽象方法的三种方式**

- 该类直接定义了抽象方法
- 该类继承了一个抽象父类，但没有完全实现父类包含的抽象方法
- 该类实现了一个接口，但没有完全实现接口包含的抽象方法

**`abstract` 修饰符语法规则**

- `abstract` 和 `private` 可以同时修饰内部类，不能同时修饰方法
  - 因为 `abstract` 修饰的方法需要被子类重写，而 `private` 修饰的方法无法被子类重写
- `abstract` 和 `static` 可以同时修饰内部类，不能同时修饰方法
  - 因为 `abstract` 修饰的方法没有方法实现，不可能被直接调用，而 `static` 修饰的方法则可以通过类直接调用
- `abstract` 和 `final` 不能同时使用
  - 因为 `abstract` 修饰的类只能被继承，而 `final` 修饰的类不能被继承
  - 因为 `abstract` 修饰的方法只能被重写，而 `final` 修饰的方法不能被重写

**抽象类的作用**

- 提供一个公共类型『可以使用抽象类作为引用变量，指向不同子类实例』
- 提取不同子类的重复内容（成员变量和方法）
- 避免无意义的父类的实例化，限制子类设计的随意性

**应用场景**：某个父类只知道其子类应该包含怎样的方法，但无法准确知道子类如何实现这些方法

## 接口

**接口体现了规范和实现分离的设计哲学**：接口定义 **规范**，而类基于接口定义的规范提供 **具体实现**，因此称之为 `实现类`

**接口的定义**

```java
[public] interface InterfaceName [extends interface1,interface2 ...]{
    /*
    * 由于接口定义的是一种规范，而非具体实现，所以不能实例化，也无需被继承
    * 因此接口不能定义构造器、初始化块、实例成员
    * 只能定义常量、类方法、类内部类、抽象方法、默认方法
    */
    public abstract 抽象方法
    public default 默认方法『有方法实现』

    public static final 成员变量 = 初始值 // 常量
    public static 类方法『有方法实现』
    public static 内部类『普通类 / 抽象类 / 接口 / 枚举类』
}

默认方法的调用规则
1. 实现类的实例成员可以直接调用接口的默认方法
2. 实现类如果重写了默认方法，则需要通过 InterfaceName.super.defaultMethodName() 的形式调用接口的默认方法
```

**接口的使用**

- 调用类的常量、类方法、类内部类（普通类 / 抽象类 / 接口 / 枚举类)
- 供其他类实现：接口的实现类可以获得接口里定义的常量、实例方法『抽象方法、默认方法』
- 声明引用变量，该变量只能引用该接口实现类的对象
- 强制类型转换

**接口的其他规则**

- **多继承**：一个接口可以继承多个直接父接口，子接口将获得父接口里定义的所有抽象方法、常量
- 接口声明的引用变量在调用常量时优先使用接口的常量
- 多接口重名默认方法解决方案：若接口实现类的父类定义了公有的同名方法，则可以直接使用父类的方法，否则必须重写该方法，因为编译器无法确定实现类将使用哪个接口的方法
- 多接口重名常量解决方案：若自定义同名变量，则可以直接使用自定义的变量，否则需要明确调用者才能调用父类或接口的常量

## 接口和抽象类

**抽象类定义模板，接口定义规范**

- **抽象类** 体现了模板式设计，抽象类作为子类的模板，是一个 `实现了通用方法，定义了抽象方法` 的中间产品，需要子类进行完善
- **接口** 体现了规范式设计，接口只定义实现类的规范，是一个抽象的更加彻底的"抽象类"，需要实现类完全实现其定义的规范
  - 对于接口的实现者：接口规定实现者必须向外提供的服务『以方法的形式提供』
  - 对于接口的调用者：接口规定调用者可以调用的服务以及如何调用
  - 对于整个应用程序：接口定义了模块间的耦合标准、通信标准