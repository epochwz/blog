# 调用构造器创建Bean实例
设值注入
- Spring容器使用默认构造器创建Bean实例,则对所有实例变量执行默认初始化[0,false,null]
- 根据配置文件决定依赖关系,先实例化被依赖的Bean实例,然后为Bean注入依赖关系,最后返回完整的Bean实例

构造注入:Spring容器调用带对应参数的构造器来创建Bean实例,传入的参数即用于初始化Bean的实例变量，最后也返回完整的Bean实例

必须指定class属性[Bean实例的实现类]

# 调用静态工厂方法创建Bean
1. 定义接口`Being.java`
    ```java
    package org.jwz.factory;

    public interface Being {
        void testBeing();
    }

    ```
2. 定义实现类`Cat.java`和`Dog.java`
    ```java
    package org.jwz.factory.impl;

    import org.jwz.factory.Being;
    
    public class Cat implements Being {
        private String msg;
    
        public void setMsg(String msg) {
            this.msg = msg;
        }
    
        @Override
        public void testBeing() {
            System.out.println(msg+",猫爱吃鱼");
        }
    }
    ```
    ```java
    package org.jwz.factory.impl;

    import org.jwz.factory.Being;
    
    public class Dog implements Being {
        private String msg;
    
        public void setMsg(String msg) {
            this.msg = msg;
        }
    
        @Override
        public void testBeing() {
            System.out.println(msg+",狗爱吃骨头");
        }
    }
    ```
3. 定义静态工厂类
    ```java
    package org.jwz.factory;

    import org.jwz.factory.impl.Cat;
    import org.jwz.factory.impl.Dog;
    
    // 静态工厂类
    public class BeingFactory {
        // 静态工厂方法,根据传入参数返回对应的Bean实例
        public static Being getBeing(String arg) {
            if (arg.equalsIgnoreCase("dog")) {
                return new Dog();
            }else{
                return new Cat();
            }
        }
    }
    ```
4. 配置Bean[静态工厂类]
    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    
        <!--使用指定的静态工厂类的静态工厂方法来创建dog Bean实例-->
        <bean id="dog" class="org.jwz.factory.BeingFactory" factory-method="getBeing">
            <!--为静态工厂方法传入参数-->
            <constructor-arg value="dog" />
            <!--为dog Bean的setter方法传入参数-->
            <property name="msg" value="我是狗" />
        </bean>
        <bean id="cat" class="org.jwz.factory.BeingFactory" factory-method="getBeing">
            <constructor-arg value="cat" />
            <property name="msg" value="我是猫" />
        </bean>
    </beans>
    ```

# 调用实例工厂方法创建Bean
1. 将静态工厂方法改成实例工厂方法`public Being getBeing(String arg)`
2. 配置BeingFactroy Bean`<bean id="beingFactory" class="org.jwz.factory.BeingFactory"/>`
3. 使用factory-bean属性指定实例工厂Bean:`<bean id="dog" factory-bean="beingFactory" factory-method="getBeing">`