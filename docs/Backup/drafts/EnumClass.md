# 枚举类

## 普通枚举类

**枚举类：**使用 `enum` 关键字定义的特殊类，地位等同于类 / 接口

* 枚举类默认继承了 `java.lang.Enum` 类，因此不能显式继承其他父类
* 枚举类无法被外部类继承
* 枚举类只能在枚举类内部显式地创建实例：首行列出枚举类所有实例，且系统会默认添加 `public static final`修饰枚举类实例
* 枚举类构造器只能使用 private 访问控制符
* 枚举类默认提供了一个 values() 方法，相当于该类实例集合的迭代器

**枚举类通常应该设计成不可变类：**不允许改变枚举类实例的状态数据

```java
// 完善的枚举类定义
enum Gender{
    //
    // 首行列举枚举类实例，相当于定义 final 类变量保存实例并初始化
    // public static final Gender MALE=new Gender("男");
    MALE("男"),FEMALE("女");

    // 定义 final 实例变量，防止重新赋值
    private final String name;
    // 隐藏构造器避免随意创建实例
    private Gender(String name){
        this.name = name;
    }
    // 仅提供 getter，不提供 setter
    public String getName(){
        return this.name;
    }
}

public class GenderBestTest{
    public static void main(String[] args){
        // 只能获取实例，并通过实例 getter 方法获取信息，无法进行任何修改操作
        Gender g = Gender.valueOf("MALE");
        System.out.println(g + "代表：" + g.getName());
    }
}
```

## 抽象枚举类

**抽象枚举类**：定义了抽象方法的枚举类，不能用 abstract 修饰符，只能通过定义抽象方法使系统自动为其添加 abstract 定义

* 抽象枚举类默认使用`abstract 修饰符`，而非抽象的枚举类默认使用`final 修饰符`
* 抽象枚举类本身无法实例化，因此每一个枚举值相当于抽象枚举类的匿名子类的实例，必须为抽象方法提供实现，否则编译出错

```java
// 定义抽象枚举类
public enum AbstractEnumTest{
    PLUS{
        public double eval(double x , double y){
            return x + y;
        }
    },
    MINUS{
        public double eval(double x , double y){
            return x - y;
        }
    },
    TIMES{
        public double eval(double x , double y){
            return x * y;
        }
    },
    DIVIDE{
        public double eval(double x , double y){
            return x / y;
        }
    };

    // 为枚举类定义一个抽象方法，由不同的枚举值提供不同的实现
    public abstract double eval(double x, double y);

    public static void main(String[] args){
        System.out.println(AbstractEnumTest.PLUS.eval(5, 4));
        System.out.println(AbstractEnumTest.MINUS.eval(5, 4));
        System.out.println(AbstractEnumTest.TIMES.eval(5, 4));
        System.out.println(AbstractEnumTest.DIVIDE.eval(5, 4));
    }
}
```

## 接口枚举类

**接口枚举类**：实现了接口的枚举类

* 如果由枚举类来实现接口里的方法，则每个枚举值调用该方法的行为方式都一样。
* 若要呈现不同行为方式，则可以为每个枚举值提供不同的方法实现
* 此时枚举值相当于枚举类的匿名实现类的实例，而非此枚举类的实例

```java
package jwz;

// 定义接口：枚举类实例的方法
interface GenderDesc{
  void info();
}

// 定义枚举类，实现 GenderDesc 接口
enum Gender implements GenderDesc{
  // 列举枚举类实例并调用构造器初始化实例信息
  // 相当于创建一个匿名内部类，而枚举类实例指向该匿名内部类
  // new Gender("男"){
  //    public void info(){
  //        System.out.println("这个枚举值代表男性");
  //    }
  // }
  MALE("男"){// 花括号部分实际上是一个类体部分
      public void info(){
          System.out.println("这个枚举值代表男性");
      }
  },
  FEMALE("女"){
      public void info(){
          System.out.println("这个枚举值代表女性");
      }
  };
  // 定义 final 实例变量
  private final String name;
  // 隐藏枚举类构造器
  private Gender(String name){
      this.name = name;
  }
  // 提供 getter 获取信息
  public String getName(){
      return this.name;
  }
  // 实现 GenderDesc 接口必须实现的方法
  public void info(){
      System.out.println("枚举类对接口方法的默认实现");
  }
}

// 测试实现接口的枚举类
public class GenderInterfaceTest{
  public static void main(String[] args) {
      Gender.MALE.info();
  }
}
```

## 枚举

**枚举**：实例有限且固定的一类事物（例如四季只有春夏秋冬、性别只有男女）

**枚举的实现方式**

1. 定义 静态常量 来表示枚举
2. 自定义 实例有限且固定 的 不可变类
3. 使用 enum 关键字定义的枚举类

## 通过静态常量实现的枚举

```java
// 定义了性别枚举『男，女』
public class Gender{
    public static final String MALE="MALE";
    public static final String FEMALE="FEMALE";
}
```

## 通过自定义不可变类实现的枚举

```java
package jwz;

/**
 * 自定义多例不可变类实现枚举类效果
 */
class Season{
    // 列举该类所有实例
    public static final Season SPRING = new Season("春天" , "趁春踏青");
    public static final Season SUMMER = new Season("夏天" , "夏日炎炎");
    public static final Season FALL = new Season("秋天" , "秋高气爽");
    public static final Season WINTER = new Season("冬天" , "围炉赏雪");

    // 定义私有实例变量
    private final String name;
    private final String desc;

    // 只为 name 和 desc 提供 getter 方法
    public String getName(){
        return this.name;
    }
    public String getDesc(){
        return this.desc;
    }

    // 隐藏构造器，防止随意创建对象
    private Season(String name , String desc){
        this.name = name;
        this.desc = desc;
    }
}

/**
 * 测试：自定义不可变类实现的枚举类
 */
public class SeasonImmutableTest{
    public SeasonImmutableTest(Season s){
        System.out.println(s.getName() + "，这真是一个"+ s.getDesc() + "的季节");
    }
    public static void main(String[] args){
        // 直接使用 Season 的 FALL 常量代表一个 Season 实例
        new SeasonImmutableTest(Season.FALL);
    }
}
```