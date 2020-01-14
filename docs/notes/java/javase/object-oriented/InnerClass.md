# 内部类

**定义**：在某个类的内部定义的类，称为**内部类**。反之，包含内部类的类称为**外部类**

**作用**：把内部类的信息隐藏在外部类之内，不允许其他类随意访问，从而实现更好的封装

**分类**

```bash
成员内部类：一种与成员变量、方法、构造器、初始化块相似的类的成员
    静态内部类：static 修饰的内部类，属于外部类本身，不属于外部类实例
    非静态内部类：非 static 修饰的内部类，属于外部类实例
局部内部类：定义在外部类的方法 / 代码块里的类
匿名内部类：没有类名的局部内部类，在定义类的同时创建类的实例，因此只能实例化一次
```

**内部类编译后产生的类文件格式**

- 静态 / 非静态内部类：`OuterClass$InnerClass.class`
- 局部内部类：`OuterClass$NumberInnerClass.class`
- 匿名内部类 `OuterClass$Number.class`

## 静态内部类

静态内部类可以拥有实例成员，也可以拥有类成员

### 外部类内部访问静态内部类

外部类的实例成员和类成员都可以直接访问静态内部类：因为静态内部类就相当于外部类的一个类成员，因此外部类的实例成员和类成员都可以直接访问它

- 创建实例：`InnerClass innerInstance=new InnerClass();`
- 访问成员：外部类不能直接访问静态内部类的实例成员，只能通过创建静态内部类的实例来访问
  - 访问类成员：`InnerClass.staticMember`
  - 访问实例成员：`innerInstance.nonStaticMember`

### 外部类外部访问静态内部类

外部类以外的类可以通过 `OuterClass.InnerClass` 的形式直接访问静态内部类

- 创建实例：`OuterClass.InnerClass innerInstance=new OuterClass.InnerClass()`
- 访问成员：外部类以外的类不能直接访问静态内部类的实例成员，只能通过创建静态内部类的实例来访问
  - 访问类成员 `OuterClass.InnerClass.staticMember`
  - 访问实例成员 `innerInstance.nonStaticMember`
- 继承：`class SubStaticInnerClass extends OuterClass.InnerClass{}`

### 静态内部类访问外部类

#### 静态内部类访问外部类的类成员

静态内部类的实例成员和类成员都可以直接访问外部类的类成员

**静态内部类同名变量**

- 可以通过类名调用明确同名变量的调用者
- 如果不明确指定同名变量的调用者，将按此顺序查找并调用：`方法中的局部变量 -> 内部类的成员变量 -> 直接父类中的成员变量 -> 上溯至 Object -> 外部类中的成员变量 -> 外部类直接父类中的成员变量 -> 上溯至 Object -> 编译出错`

**静态内部类同名方法**

- 可以通过类名调用明确同名方法的调用者
- 如果不明确指定同名方法的调用者，将按此顺序查找并执行：`内部类类方法 -> 内部类父类类方法 -> 上溯至 Object -> 外部类类方法 -> 外部类父类类方法 -> 上溯至 Object -> 编译出错`

#### 静态内部类访问外部类的实例成员

静态内部类不能直接访问外部类的实例成员，只能通过创建外部类的实例对象来访问：因为静态内部类相当于外部类的一个类成员，类成员不能直接访问实例成员

## 非静态内部类

非静态内部类只能拥有实例成员，不能拥有类成员

### 外部类访问非静态内部类

外部类的类成员不能直接访问非静态内部类：因为非静态内部类是外部类的实例成员，而类成员不能访问实例成员，所以外部类的类成员必须通过创建一个外部类实例来访问非静态内部类

- 创建实例：`InnerClass innerInstance=outerInstance.new InnerClass()`
- 访问成员：`innerInstance.nonStaticMember`

外部类的实例成员可以直接访问非静态内部类：因为外部类的实例成员持有外部类的实例引用，可以直接访问同为实例成员的非静态内部类

- 创建实例：`InnerClass innerInstance=new InnerClass();`
- 访问成员：`innerInstance.nonStaticMember`

### 外部类外部访问非静态内部类

外部类以外的类只能通过外部类的实例访问其非静态内部类

- 创建实例 `OuterClass.InnerClass innerInstance=outerInstance.new InnerClass()`
- 访问成员：`innerInstance.nonStaticMember`
- 继承

    ```java
    public class OuterClass{
        class InnerClass{
            public InnerClass(params){}
        }
    }
    public class SubNonStaticInnerClass extends OuterClass.InnerClass{
        SubNonStaticInnerClass(OuterClass instance){
            instance.super(params);
        }
    }
    ```

### 非静态内部类访问外部类

非静态内部类可以直接访问外部类的类成员和实例成员：因为非静态内部类相当于外部类的一个实例成员，总是持有其外部类实例的引用 `OuterClass.this`；实例成员可以访问其他实例成员，也可以直接访问类成员

**非静态内部类同名变量**

- 可以通过限定符明确变量的调用者
  - 局部变量：直接访问
  - 内部类实例成员变量：`this.varName`
  - 内部类父类实例成员变量 `super.varName`
  - 内部类父类类成员变量 `SuperClass.varName`
  - 外部类实例成员变量：`OuterClass.this.varName`
  - 外部类类成员变量：`OuterClass.varName`
- 如果不明确指定同名变量的调用者，将按此顺序查找并调用：`方法中的局部变量 -> 内部类的实例成员变量 -> 直接父类中的成员变量 -> 上溯至 Object -> 外部类中的成员变量 -> 外部类直接父类中的成员变量 -> 上溯至 Object -> 编译出错`

**非静态内部类同名方法**

- 可以通过限定符明确方法的调用者
  - 内部类实例方法：直接访问
  - 内部类父类实例方法 `super.method()`
  - 内部类父类类方法 `SuperClass.method()`
  - 外部类实例方法：`OuterClass.this.method()`
  - 外部类类方法：`OuterClass.method()`
- 如果不明确指定同名方法的调用者，将按此顺序查找并执行：`内部类实例方法 -> 直接父类的类方法 / 实例方法 -> 上溯至 Object -> 外部类的类方法 / 实例方法 -> 上溯至 Object -> 编译出错`

## 局部内部类

**定义**：定义在外部类的方法 / 代码块里的类

- 无法使用访问权限修饰符
- 不能定义静态成员
- 使用局部内部类定义变量、创建实例、派生子类，都只能在局部内部类所在方法 / 代码块内进行

**局部内部类如果要访问外部定义的局部变量，则该局部变量只能是常量**

- Java8 之前，要求局部内部类访问的外部的局部变量必须使用 final 修饰
- Java8 之后，该限制被取消：若外部的局部变量被局部内部类访问，则系统自动使用 final 修饰该局部变量
- 代码示例

    ```java
    class OuterClass {
        public static void main(String[] args) {
            int age = 10;
            class LocalInnerClass {
                void info() {
                    // 在 Java 8 以前，在局部内部类中访问外部非 final 局部变量，将直接提示编译错误：age 必须使用 final 修饰
                    // 从 Java 8 开始，匿名内部类、局部内部类允许访问外部非 final 的局部变量：自动将其变成 final 变量
                    System.out.println(age);
                }
            }
            // 如果写上此行代码，则匿名内部类中访问 age 的代码将会编译出错
            // 因为局部内部类访问的外部的局部变量必须是常量，不能重新赋值，所以此代码与内部类中访问 age 的代码互相矛盾
            // age = 1;
        }
    }
    ```

## 匿名内部类

**匿名内部类**：没有名字的 `局部内部类`。

- 没有类名
- 无法使用访问权限修饰符
- 不能定义静态成员
- 无法使用 `abstract` 修饰符
- 无法定义构造器『因为没有类名，无法定义』，但可以使用初始化代码块
- 要么继承一个父类，要么实现一个接口，且必须实现父类 / 接口的所有抽象方法

**无法重复实例化**：匿名内部类自带类的实现体，定义匿名内部类时就会立即创建一个匿名内部类的实例，然后这个类定义立即消失，因此只能创建一个实例

**适用场景**

- 只需要类的一个实例且只需要用到一次的类
- 给类命名并不能够让代码变得更加容易理解
