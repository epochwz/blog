## 6.8.1 Lambda表达式入门

**Lambda表达式：**支持将代码块作为方法参数。通常用于代替匿名内部类的繁琐语法，,创建函数式接口的实例

* **函数式接口：**只包含一个抽象方法的接口\[但可以包含多个默认方法、类方法\]
* @FunctionalInterface 注解，放在接口定义前面，告诉编译器执行更严格检查----必须是函数式接口，否则编译报错

Lambda表达式是匿名内部类的简化，无须new，也不需要指出重写方法的名字，也无须给出方法的返回值类型----只需给出重写的方法的形参列表和方法体即可。

用法示例:

```java
package jwz;

// 定义函数式接口
interface Command{
    // 接口里定义的process()方法用于封装数组的“处理行为”
    void process(int[] target);
}

// 定义处理数组的类
class ProcessArray{
    public void process(int[] target , Command cmd){
        cmd.process(target);
    }
}

public class CommandTest{
    // 使用匿名内部类来封装处理行为
    public static void innerClassTest(){
        // 定义要处理的数组
        int[] array = {3, -4, 6, 4};
        ProcessArray pa = new ProcessArray();
        // 调用处理数组的方法
        // 具体处理行为取决于匿名内部类
        pa.process(array , new Command(){
            public void process(int[] target){
                int sum = 0;
                for (int tmp : target ){
                    sum += tmp;
                }
                System.out.println("数组元素的总和是:" + sum);
            }
        });
    }

    // 使用Lambda表达式代替匿名内部类来封装处理行为
    public static void lambdaTest(){
        // 定义要处理的数组
        int[] array = {3, -4, 6, 4};
        ProcessArray pa = new ProcessArray();
        // 调用处理数组的方法
        // 具体处理行为取决于匿名内部类->Lambda实现的函数式接口
        pa.process(array , (int[] target)->{
            int sum = 0;
            for (int tmp : target ){
                sum += tmp;
            }
            System.out.println("数组元素的总和是:" + sum);
        });
    }
    public static void main(String[] args){
        CommandTest.innerClassTest();
        CommandTest.lambdaTest();
    }
}
```

Lambda由三部分组成：

* 形参列表：允许省略形参类型，若形参列表只有一个参数，甚至连形参列表的圆括号也可以省略
* 箭头：必须通过英文中画线和大于符号组成
* 代码块：
  * 若代码块只包含一条语句，允许省略花括号。
  * 若代码块只有一条return语句，甚至可以省略return关键字。\[Lambda 表达式需要返回值，而它的代码块中仅有一条省略了return的语句，Lambda 表达式会自动返回这条语句的值\]

Lambda表达式简化写法示例:

```java
package jwz;

interface Eatable{
    void taste();
}
interface Flyable{
    void fly(String weather);
}
interface Addable{
    int add(int a , int b);
}

public class LambdaQs{
    // 调用该方法需要Eatable对象
    public void eat(Eatable e){
        System.out.println(e);
        e.taste();
    }
    // 调用该方法需要Flyable对象
    public void drive(Flyable f){
        System.out.println("我正在驾驶：" + f);
        f.fly("【碧空如洗的晴日】");
    }
    // 调用该方法需要Addable对象
    public void test(Addable add){
        System.out.println("5与3的和为：" + add.add(5, 3));
    }


    public static void main(String[] args){
        LambdaQs lq = new LambdaQs();
    // Lambda表达式 会被当成一个"任意类型"的对象，其"目标类型"取决于形参类型
        // 1. Lambda表达式的代码块只有一条语句，可以省略花括号。
        lq.eat(()-> System.out.println("苹果的味道不错！"));

        // 2. Lambda表达式的形参列表只有一个形参，省略圆括号
        lq.drive(weather ->{
            System.out.println("今天天气是：" + weather);
            System.out.println("直升机飞行平稳");
        });

        // 3. Lambda表达式的代码块只有一条语句，省略花括号
        // 4. 代码块中只有一条语句，即使该表达式需要返回值，也可以省略return关键字。
        lq.test((a , b)->a + b);
    }
}
```

## 6.8.2 Lambda 表达式与函数式接口

Lambda 表达式的结果被当成一个对象,可用于赋值。
Lambda 表达式的类型称为"目标类型"，取决于调用方法的形参类型,必须是"函数式接口"。

三种保证 Lambda 表达式的目标类型是明确的函数式接口的常见方式：

* 将其赋值给函数式接口类型的 变量
* 将其作为函数式接口类型的参数传给某个方法
* 使用函数式接口对其进行强制类型转换

Lambda 表达式的目标类型完全可能是变化的----唯一要求是，Lambda表达式实现的匿名方法与目标类型中唯一的抽象方法有相同的形参列表

```java
package jwz;

// 定义函数式接口并使用注解检查是否合法
@FunctionalInterface
interface FkTest{
    void run();
}

public class LambdaTest{
    public static void main(String[] args){
        // Runnable接口中只包含一个无参数的方法，是函数式接口
        // Lambda表达式代表的匿名方法实现了Runnable接口中唯一的、无参数的方法，相当于创建了一个Runnable对象
        Runnable r = () -> {
            for(int i = 0 ; i < 10 ; i ++){
                System.out.print(i+"\t");
            }
            System.out.println();
        };

//        // 下面代码报错: 不兼容的类型: Object不是函数接口
//        Object obj = () -> {
//            for(int i = 0 ; i < 10 ; i ++){
//                System.out.print(i+"\t");
//            }
//            System.out.println();
//        };

    // 同样的Lambda表达式可以被当成不同的目标类型，唯一的要求是：Lambda表达式的形参列表与函数式接口中唯一的抽象方法的形参列表相同
        // 通过强制类型转换确保Lambda表达式的目标类型是函数式接口
        Object obj1 = (Runnable)() -> {
            for(int i = 0 ; i < 10 ; i ++){
                System.out.print(i+"\t");
            }
            System.out.println();
        };
        // 通过强制类型转换确保Lambda表达式的目标类型是函数式接口
        Object obj2 = (FkTest)() -> {
            for(int i = 0 ; i < 10 ; i ++){
                System.out.print(i+"\t");
            }
            System.out.println();
        };

    // 分别调用函数式接口对象的方法，方法执行体由上述的Lambda表达式决定
        r.run();
        ((Runnable)obj1).run();
        ((FkTest)obj2).run();
    }
}
```

## 方法引用与构造器引用

若Lambda表达式的代码块只有一条代码，程序可以省略其花括号，还可以在代码块中使用方法引用和构造器引用

```java
package jwz;

import javax.swing.*;

@FunctionalInterface
interface Converter{
    // 该方法负责将String参数转换成Integer
    Integer convert(String from);
}
@FunctionalInterface
interface MyTest{
    // 该方法实现截取字符串a从b到c这一段字符
    String test(String a , int b , int c);
}
@FunctionalInterface
interface YourTest{
    // 该方法负责创建标题为title的窗口，并将该窗口对象作为返回值
    JFrame win(String title);
}

public class MethodRefer{
    public static void main(String[] args){
    // 1. 引用类方法
        // 1. Lambda表达式创建Converter对象
        Converter converter1 = from -> Integer.valueOf(from);
        // 调用Converter1对象将字符串转换成整数
        System.out.println(converter1.convert("99")); // 输出整数99

        // 2. 方法引用代替Lambda表达式创建Converter对象:引用类方法
        // 函数式接口Converter中被实现方法的全部参数from被传给该类方法valueOf作为参数。
        Converter converter2 = Integer::valueOf;
        // 调用Converter2对象将字符串转换成整数
        System.out.println(converter2.convert("99")); // 输出整数99


    // 2. 引用特定对象的实例方法
        // 1. Lambda表达式创建Converter对象
        Converter converter3 = from -> "fkit.org".indexOf(from);
        System.out.println(converter3.convert("t")); // 输出2

        // 2. 方法引用代替Lambda表达式创建Converter对象：引用特定对象的实例方法。
        // 函数式接口中被实现方法的全部参数传给该方法作为参数。
        Converter converter4 = "fkit.org"::indexOf;
        System.out.println(converter4.convert("t")); // 输出2

    // 3. 引用某类对象的实例方法

        // 1. Lambda表达式创建MyTest对象
        MyTest mt1 = (a , b , c) -> a.substring(b , c);
        System.out.println(mt1.test("Java I Love you" , 2 , 9)); // 输出:va I Lo

        // 2. 方法引用代替Lambda表达式：引用某类对象的实例方法。
        // 函数式接口中被实现方法的第一个参数作为调用者，后面的参数全部传给该方法作为参数。
        MyTest mt2 = String::substring;
        System.out.println(mt2.test("Java I Love you" , 2 , 9)); // 输出:va I Lo


    // 4. 引用构造器
        // Lambda表达式 创建YourTest对象
        YourTest yt1 = (String a) -> new JFrame(a);
        // 构造器引用 创建YourTest对象：函数式接口中被实现方法的全部参数传给该构造器作为参数。
        YourTest yt2 = JFrame::new;
        System.out.println(yt2.win("我的窗口"));
    }
}
```

## Lambda 表达式与匿名内部类的联系和区别

Lambda 表达式是匿名内部类的一种简化，可以部分取代匿名内部类的作用。

相同点：

* 可以直接访问"effectively final"的局部变量，以及外部类的成员变量\[包含实例变量和类变量\]
* 创建的对象可以调用从接口中继承的默认方法

区别

匿名内部类实现的抽象方法允许直接调用接口中定义的默认方法，而 Lambda 表达式实现的抽象方法只能通过其返回的对象来调用。
