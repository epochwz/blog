# 单例模式

## 定义

单例模式 (Singleton) 保证一个类在整个系统中有且只有一个实例，且必须自行实例化并提供一个全局访问点供整个系统访问这个实例

## 目的

目的：有时候我们希望某个类在整个系统中只存在一个唯一的实例，这样既节省了内存空间，又有利于我们协调系统的整体行为。

- 创建对象时占用资源过多，但同时又需要用到该类对象
- 需要对系统内的资源进行统一读写。例如，读写配置信息
- 当多个实例存在可能引起程序逻辑错误，例如 ID 生成器
- 类似的应用场景还有线程池、连接池、计数器、缓存、日志、打印机、驱动程序等等。

## 优点

- 单例类在内存中只存在一个实例
  - 减少了内存占用
  - 避免对共享资源的多重占用
- 单例类仅提供一个全局访问点
  - 封装性强，容易使用
  - 有利于协调系统的整体行为

## 缺点

- 单例类没有接口，后续扩展困难
- 如果实例化后的对象长期不使用，可能会被垃圾回收，造成对象状态丢失

## 实现

单例类的实现并不复杂，只需要关注以下几个要点

1. 私有构造器：禁止外部随意创建对象
2. 全局访问点：提供外部访问实例的途径
3. 是否需要 **延迟初始化** 功能（当系统第一次明确获取单例类的实例时，才开始初始化实例）
   - 饿汉式：不具备延迟初始化功能，不存在线程安全问题，实现简单
     - 在类加载时就被初始化，如果系统从始至终都没有使用到该类，则会造成资源浪费
       <!-- TODO 如果系统不使用该类的话，这个类不就不会被初始化吗？ -->
   - 懒汉式：具备延迟初始化功能，存在线程安全问题，实现稍微复杂
4. 保证线程安全
5. 防止单例破坏

### 饿汉式

饿汉式单例模式的实现代码如下

```java
public class HungrySingleton {
    // 饿汉式：在类初始化的时候就创建这个实例
    private static final HungrySingleton instance = new HungrySingleton();

    // 1. 私有构造器
    private HungrySingleton() { }

    // 2. 全局访问点
    public static HungrySingleton getInstance() {
        return instance;
    }
}
```

测试代码如下

```java
public class Test {
    public static void main(String[] args) {
        // HungrySingleton hungrySingleton = new HungrySingleton(); // 编译错误
        HungrySingleton instance1 = HungrySingleton.getInstance();
        HungrySingleton instance2 = HungrySingleton.getInstance();
        System.out.println(instance1 == instance2); // true
    }
}
```

### 懒汉式

懒汉式单例模式的实现代码如下

```java
public class LazySingleton {
    // 懒汉式：在声明实例的时候不急着创建实例
    private static LazySingleton lazySingletonInstance;

    // 1. 私有构造器
    private LazySingleton() { }

    // 2. 全局访问点
    public static LazySingleton getInstance() {
        // 懒汉式：在外部第一次想要获取实例的时候，再进行创建
        if (lazySingletonInstance == null) {
            lazySingletonInstance = new LazySingleton();
        }
        return lazySingletonInstance;
    }
}
```

懒汉式单例模式的测试代码和饿汉式一样，就不重复了

## 线程安全

在上一节中，我们已经实现了饿汉式和懒汉式两种写法的单例模式。在此基础上，我们来分析一下两种写法是否能够保证线程安全。

### 饿汉式线程安全

对于饿汉式单例类 `HungrySingleton`，当某个线程首次调用 `getInstance` 方法时，`HungrySingleton` 类会被初始化，此时 JVM 会给 Class 对象加锁，初始化完成时唯一的单例类实例 `instance` 已被创建完成，因此后续线程拿到的都是初始化时已创建好的的同一个实例，因此饿汉式单例模式是线程安全的

![HungrySingleton-SequenceDiagram][]

### 懒汉式线程不安全

对于懒汉式单例类，在多线程的情况下，可能出现多次创建实例的情况，因此是线程不安全的。

![LazySingleton-SequenceDiagram][]

懒汉式单例类的多线程测试代码如下

```java
public class MultiThreadTest {
    public static void main(String[] args) {
        Thread threadA = new Thread(new T());
        Thread threadB = new Thread(new T());
        threadA.start();
        threadB.start();
    }
}

class T implements Runnable {
    @Override
    public void run() {
        LazySingleton instance = LazySingleton.getInstance();
        System.out.println(Thread.currentThread().getName() + ":  " + instance);
    }
}
```

在 IDE 中执行上述代码，（大概率）会发现依然没有什么问题，两个线程输出的单例类实例仍旧是同一个实例

```java
Thread-0:  com.epoch.design.pattern.creational.singleton.HungrySingleton@118f2161
Thread-1:  com.epoch.design.pattern.creational.singleton.HungrySingleton@118f2161
```

不过，这只是因为我们的单例类很简单，执行的很快，所以很难触发多线程的 Bug. 此时我们可以使用 IDEA 的[多线程 Debug 功能](IDEA-MultiThread-Debug) , 控制两个线程的执行顺序如上面时序图所示，就能够人为的触发这个 Bug 了。
<!-- TODO 如何使用 IDEA 进行多线程 Debug -->

### 解决懒汉式线程安全问题

#### synchronized

使用 `synchronized` 可以很简单地解决线程安全问题

```java
public synchronized static LazySingleton getInstance() {...}
```

![SynchronizedLazySingleton-SequenceDiagram][]

使用 `synchronized` 虽然简单，但每次调用 `getInstance()` 方法时都会进行加锁，造成不必要的开销

![TimeDiff-Lazy-And-Hungry][]

#### 双重检查锁

使用 `双重锁检查 (Double Check)` 的方法，只有在实例尚未创建完成时调用 `getInstance()` 方法时会加锁，实例创建完成后则不会再有加锁操作，性能明显优于 `synchronized`

```java
public class DoubleCheckLazySingleton {
    private static DoubleCheckLazySingleton instance;

    // 1. 私有构造器
    private DoubleCheckLazySingleton() { }

    // 2. 全局访问点
    public static DoubleCheckLazySingleton getInstance() {
        if (instance == null) {
            synchronized (DoubleCheckLazySingleton.class) {
                if (instance == null) {
                    instance = new DoubleCheckLazySingleton();
                }
            }
        }
        return instance;
    }
}
```

但在这种情况下，创建实例的这一行代码 `instance = new DoubleCheckLazySingleton();` 可能会出现 [指令重排序](/Java/JVM/Instruction-Reordering-And-Volatile.md) 的问题，从而导致实例尚未初始化完成就被使用的情况，造成系统异常。

##### 指令重排序

创建单例类实例的代码 `instance = new DoubleCheckLazySingleton();` 实际上可以分成以下几个步骤

```bash
1. 给对象分配内存
2. 初始化对象
3. 将对象内存地址赋值给 instance
```

经过指令重排序后，这几个步骤的顺序（可能）会变成

```bash
1. 给对象分配内存
3. 将对象内存地址赋值给 instance
2. 初始化对象
```

因此，如果其他线程在指令重排序后的步骤 2 和步骤 3 之间，调用了 `getInstance()` 并使用 `instance` 对象，则可能出现异常

![Instruction-Reordering][]

解决指令重排序也很简单，只需要将单例类的实例变量声明成 volatile 变量，即可禁止指令重排序：不能将访问 `instance` 变量前面的语句放到后面，也不能将后面的语句放到前面

```java
private volatile static DoubleCheckLazySingleton instance;
```

## 单例破坏

### 序列化 / 反序列化

通过序列化 / 反序列化可以创建多个单例类的实例，从而达到破坏单例模式的效果

- 前提：单例类必须实现 `Serializable` 接口，才能进行序列化和反序列化

```java
public static void main(String[] args) throws IOException, ClassNotFoundException {
    String singletonInstanceFile = "singleton_file";

    HungrySingleton instance = HungrySingleton.getInstance();

    // 序列化：将实例写入文件
    ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream(singletonInstanceFile));
    oos.writeObject(instance);

    // 反序列化：从文件中读取实例
    ObjectInputStream ois = new ObjectInputStream(new FileInputStream(singletonInstanceFile));
    Object newInstance = ois.readObject();

    // false: 说明序列化 / 反序列化创建了不同的单例对象，成功破坏了单例模式
    System.out.println(instance==newInstance);
}
```

在单例类中添加 `Object readResolve()` 方法可以防止 序列化 / 反序列化破坏单例

- 原理： `ObjectInputStream.readObject()` 反射调用了 `readResolve()` 方法，将其返回值作为反序列化出来的实例

```java
public class HungrySingleton implements Serializable {
    // ...

    // 添加该方法即可防止序列化 / 反序列化破坏单例，原理在于 ObjectInputStream.readObject 的过程中，反射调用了该方法，将其返回值作为反序列化出来的实例
    private Object readResolve() {
        return instance;
    }
}
```

### 反射

通过 反射 可以轻易的创建单例类的实例，达到破坏单例模式的效果

```java
public class DestroySingletonByReflection {
    public static void main(String[] args) throws Exception {
        HungrySingleton instance = HungrySingleton.getInstance();

        Constructor constructor = HungrySingleton.class.getDeclaredConstructor();
        constructor.setAccessible(true);
        HungrySingleton newInstance = (HungrySingleton) constructor.newInstance();

        // false: 说明序列化 / 反序列化创建了不同的单例对象，成功破坏了单例模式
        System.out.println(instance==newInstance);
    }
}
```

可以尝试通过以下方式进行防御，但事实上，这种防御方式，反射依然可以破坏：因为反射甚至可以将 `private static final` 修饰的 `instance` 修改成 `null`

```java
// 在单例类的构造器中编写相关逻辑，进行防御
private HungrySingleton() {
    if (instance != null) {
        throw new RuntimeException("Singletons prohibit reflection calls");
    }
}
```

## 其他单例模式的实现方式

### 静态内部类

使用 静态内部类 可以实现 线程安全 的**懒汉式**单例模式

```java
/**
 * 基于 静态内部类 实现的懒汉式单例模式
 * 线程安全：基于内部类的类初始化锁实现的
 */
public class StaticInnerClassLazySingleton {
    // 1. 私有构造器
    private StaticInnerClassLazySingleton() { }

    // 2. 全局访问点
    public static StaticInnerClassLazySingleton getInstance() {
        return InnerClass.instance;
    }

    private static class InnerClass {
        private static StaticInnerClassLazySingleton instance = new StaticInnerClassLazySingleton();
    }
}
```

### 枚举类

使用 枚举类 可以优雅的实现 线程安全且能够防止序列化和反射破坏 的**饿汉式**单例模式，是 《Effective Java》推荐的单例实现方式。

<!-- TODO 反射能够破坏枚举类吗 -->

```java
public enum EnumSingleton {
    INSTANCE;

    public static EnumSingleton getInstance() {
        return INSTANCE;
    }
}
```

### 线程单例

使用 ThreadLocal 可以轻易的实现“线程单例”类，该类在每一个线程里各自拥有唯一的实例，起到线程隔离的作用

```java
public class ThreadLocalSingleton {
    private static final ThreadLocal<ThreadLocalSingleton> instance = ThreadLocal.withInitial(ThreadLocalSingleton::new);

    private ThreadLocalSingleton() { }

    public static ThreadLocalSingleton getInstance() {
        return instance.get();
    }
}
```

### 容器单例

使用“容器单例类”，可以在程序启动时，对多个单例类进行统一初始化，统一管理这些单例类的实例

```java
public class ContainerSingleton {
    private static final Map<String, Object> singletonMap = new HashMap<>();

    private ContainerSingleton() { }

    public static void putInstance(String key, Object instance) {
        if (isNotBlank(key) && instance != null) {
            if (!singletonMap.containsKey(key)) {
                singletonMap.put(key, instance);
            }
        }
    }

    public static Object getInstance(String key) {
        return singletonMap.get(key);
    }
}
```

## 总结

饿汉式和懒汉式的对比

- 饿汉式：线程安全、实现简单，不具备延迟初始化的功能，可能造成资源浪费
- 懒汉式：线程不安全、实现稍微复杂，具备延迟初始化的功能，可以按需使用

几种懒汉式写法的对比

- `synchronized`：每次调用 `getInstance()` 都会加锁，开销大
- `双重锁检查`：仅在第一次调用 `getInstance()` 时 （可能）加锁，初始化完成后不再加锁，开销相对较小
  - 指令重排序问题：使用 volatile 禁止指令重排序
- `静态内部类`：仅在内部类进行类初始化时加一次锁，后续不再加锁，开销最小

[IDEA-MultiThread-Debug]:https://www.google.com/search?q=IDEA+multi+thread+debug&oq=IDEA+multi+thread+debug&aqs=chrome..69i57j0.729j0j9&sourceid=chrome&ie=UTF-8

[HungrySingleton-SequenceDiagram]:https://bed.epoch.fun/CoderNotes/DesignPattern/HungrySingleton-SequenceDiagram.png
[LazySingleton-SequenceDiagram]:https://bed.epoch.fun/CoderNotes/DesignPattern/LazySingleton-SequenceDiagram.png
[SynchronizedLazySingleton-SequenceDiagram]:https://bed.epoch.fun/CoderNotes/DesignPattern/SynchronizedLazySingleton-SequenceDiagram.png
[TimeDiff-Lazy-And-Hungry]:https://bed.epoch.fun/CoderNotes/DesignPattern/TimeDiff-Lazy-And-Hungry.png
[Instruction-Reordering]:https://bed.epoch.fun/CoderNotes/DesignPattern/Instruction-Reordering.png