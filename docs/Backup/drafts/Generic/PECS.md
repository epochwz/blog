# 类型通配符

泛型通配符      List<?> 表示跟踪泛型 List 的父类，但不能加入元素，因为无法确定集合元素的类型
上限泛型通配符  `List<? extends SuperClass>` 表示 List<SubClass>的父类，SubClass 是 SuperClass 的子类
下限泛型通配符  List<? super SubClass>

## Java 泛型中的 PECS 原则

Java 泛型中存在一个 **PECS(Producer Extends Consumer Super) 原则**

它的意思是，如果在声明泛型集合时使用了 **上限通配符**或**下限通配符**，就会导致该集合产生相应的 **读写限制**

- 如果不使用类型通配符（而是使用泛型方法），则可以同时**读取**（遍历、获取) 和**写入**（添加) 元素
- 使用了上限通配符之后，集合  `Collection<? extends Type>` 不能添加元素，只能遍历、获取元素，且获取到的元素的数据类型应该是上限通配符声明的**上限类型 Type**
- 使用了下限通配符之后，集合 `Collection<? super Type>` 不适合获取元素（获取到的元素的数据类型只能是 Object 类型), 可以添加元素，且添加的元素的数据类型应当是下限通配符声明的**下限类型 Type**及其子类

其中，**Producer/Consumer** 的概念可能会让大多数人感到困惑。 其实，"PECS"原则是从集合的角度来看的，可以分为以下两个方面

- 当你想从集合中**读取**元素的时候，集合就是 "**Producer**"（“生产”了元素，供你遍历和获取）, 必须使用 `Collection<? extends Type>` (**Producer Extends**)
- 当你想**写入**元素到集合中的时候，集合就是 "**Consumer**"（集合“消费”了你提供的元素用于添加到集合中）, 必须使用 `Collection<? super Type>` (**Consumer Super**)

还是没看懂？别急，下面就通过代码来详细解释一下 PECS 原则的含义吧。

这里先定义几个普通的实体类，后文通用

```java
class Fruit{ }
class Apple extends Fruit{ }
class Banana extends Fruit{ }
```

## 上限通配符

现在，让我们从“读取”和“写入”元素的角度来看看使用泛型的上限通配符时有什么**读写限制**

### 写入元素

使用上限通配符声明一个泛型 List, 其元素类型可以是 Fruit 或其子类，并试图往该集合中添加元素

```java
List<? extends Fruit> fruits=new ArrayList<>();
// fruits.add(new Fruit());     // 编译错误
// fruits.add(new Apple());     // 编译错误
```

从结果可以看出，我们无法往该集合中添加任何类型的元素，为什么呢？让我们再看看下面这段代码

```java
public static void write(List<? extends Fruit> fruits){
    fruits.add(new Apple());  // ① 假设可以添加元素，那么添加一个 Apple
}

write(new ArrayList<Apple>()); // ② 集合元素类型必须是 Apple
write(new ArrayList<Banana>()); // ③ 集合元素类型必须是 Banana
```

在上面代码中，方法参数 fruits 表示一个元素类型是 Fruit 或其子类的泛型 List；显然，`ArrayList<Apple>` 和`ArrayList<Banana>` 都符合要求，因此代码②和③是正确的；假设代码①正确的话，那么调用代码③的时候，就会出现往 List<Banana>中添加 Apple 的情况了，显然假设是不成立的。

因此，使用了上限通配符的泛型集合`Collection<? extends Type>`是无法写入元素的，因为无法确定集合元素的具体数据类型。

### 读取元素

声明一个遍历和获取集合元素的方法，方法形参是一个使用了上限通配符的泛型 List, 其元素类型可以是 Fruit 或其子类

```java
public static void read(List<? extends Fruit> fruits) {
    for (Fruit fruit : fruits) {
        System.out.println(fruit);
    }
    Fruit fruit = fruits.get(0);
    // Apple fruit = fruits.get(0); // ① 编译错误
    // List<? extends Fruit> 的含义是 fruits 的元素是 Fruit 或其子类，因此应该用父类（声明的上限类型) Fruit 来接收，才能保证类型安全
}
```

从结果可以看出，我们可以安全的对该集合进行遍历、获取等“读取”操作。

值得注意的是，从`Collection<? extends Type>`中“读取”的集合元素的类型应该是其**上限类型 Type**

## 下限通配符

现在，让我们从“读取”和“写入”元素的角度来看看使用泛型的下限通配符时有什么**读写限制**

### 下限写入元素

```java
List<? super Fruit> fruits = new ArrayList<>();
// fruits.add(new Object()); // 编译错误
fruits.add(new Fruit());
fruits.add(new Apple());
```

[What is PECS(Producer Extends Consumer Super)](http://stackoverflow.com/questions/2723397/what-is-pecs-producer-extends-consumer-super/2723538#2723538)

[Java 泛型中的 PECS 原则](https://blog.51cto.com/flyingcat2013/1616068)

[Java 泛型中 extends 和 super 的区别？](https://itimetraveler.github.io/2016/12/27/%E3%80%90Java%E3%80%91%E6%B3%9B%E5%9E%8B%E4%B8%AD%20extends%20%E5%92%8C%20super%20%E7%9A%84%E5%8C%BA%E5%88%AB%EF%BC%9F/)