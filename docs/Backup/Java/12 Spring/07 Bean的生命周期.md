Spring容器可以管理singleton作用域的Bean的生命周期,精确的控制该Bean的创建、初始化和销毁。
- 创建Bean之后进行某些通用资源的申请,销毁Bean实例之前回收某些资源[数据库连接]

1. 通过配置Bean时指定init-method/destroy-method属性
    ```xml
    <!--
        init-method属性指定该Bean初始化完成后执行的方法
        destroy-method属性指定该Bean实例销毁之前执行的方法
        代码无须与Spring接口耦合,代码污染小
    -->
    <bean id="beanLife" class="org.jwz.life.BeanLife" init-method="init" destroy-method="myDestroy">
    ```
2. 实现InitializingBean/DisposableBean接口
    - Spring容器会检查容器中的所有Bean,若发现Bean实现了上述接口,就在为该Bean注入依赖关系之后/该Bean实例销毁之前自动调用接口方法
    - 基于Web的ApplicationContext实现中,系统已经提供了相应代码保证关闭Web应用时恰当的关闭Spring容器
    - 在独立应用中,为了让Spring容器关闭时调用singleton Bean的回调方法,必须向JVM注册一个关闭钩子(shutdown hook)
        - AbstractApplicationContext.registerShutdownHook()

# 协调作用域不同步的Bean
Bean作用域不同步现象:当singleton Bean依赖prototype Bean时,Spring容器只会向singleton Bean注入一次prototype Bean,从而导致通过singleton Bean总是只能访问到最初那个prototype Bean
- 放弃依赖注入:singleton Bean每次调用prototype Bean时主动向容器请求新的Bean实例
- 使用方法注入:lookup方法注入可以让Spring容器重写容器中Bean的抽象或具体方法,返回查找容器中其他Bean的结果,
    - Spring容器通过JDK动态代理或cglib库修改客户端的二进制代码实现上述要求

1. 将调用者Bean的实现类定义为抽象类,并定义一个抽象方法来获取被依赖的Bean
    ```java
    package org.jwz.demo.person.impl;

    import org.jwz.demo.animal.Animal;
    import org.jwz.demo.person.Person;
    
    public abstract class Chinese implements Person {
        private Animal animal;
    
        public abstract Animal getAnimal();
    
        @Override
        public void hunt() {
            System.out.println("我带着: " + getAnimal() + "出去打猎");
            getAnimal().run();
        }
    }
    ```
2. 通过lookup-method属性驱动Spring实现该Bean抽象的方法
    ```xml
    <bean id="chinese" class="org.jwz.demo.person.impl.Chinese">
        <!--驱动Spring为name属性指定的方法提供实现体,从而使抽象方法变成具体方法、抽象类变成具体类,则Spring可以创建该Bean实例-->
        <lookup-method name="getAnimal" bean="dog"/>
    </bean>
    ```

SpEL可以独立于Spring容器使用--只是当成简单的表达式使用;也可以在Annotation或XML配置文件中使用以简化Spring的Bean配置

