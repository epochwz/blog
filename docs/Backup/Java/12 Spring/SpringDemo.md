# Spring下载和配置
1. [下载Spring](https://repo.spring.io/release/org/springframework/spring "spring-framework-4.3.9.RELEASE-dist.zip")
2. 解压得到如下目录结构
    目录 | 功能
    ---|---
    docs | Spring相关文档[开发指南、API]
    schemas | Spring各种配置文件的XML Schema文档
    libs | 三种类型的JAR包
    
    JAR包后缀 | 文件类型 
    ---|--- 
    无后缀 | class文件的压缩包[**开发所需JAR包**]
    -source | 源文件压缩包
    -javadoc | API文档压缩包
3. [下载common-logging-1.2-bin.zip](http://commons.apache.org/proper/commons-logging/download_logging.cgi "Spring核心容器依赖的JAR包")
4. 添加上述JAR包到项目的类加载路径即可
    - 通过IDE/Ant/Maven/Gradle自动部署
    - 通过添加JAR包到环境变量

# Spring基本开发步骤
1. 新建项目`SpringDemo`
2. 添加Spring核心JAR包到其类加载路径
3. 定义接口`Axe.java`和`Person.java`
    ```java
    package org.jwz.demo;

    public interface Axe {
        String chop();
    }

    ```
    ```java
    package org.jwz.demo;

    public interface Person {
        void useAxe();
    }
    ```
4. 定义Axe接口的实现类`StoneAxe.java`和`SteelAxe.java`
    ```java
    package org.jwz.demo.impl;

    import org.jwz.demo.Axe;
    
    public class StoneAxe implements Axe {
        @Override
        public String chop() {
            return "使用石斧砍柴!";
        }
    }
    ```
    ```java
    package org.jwz.demo.impl;

    import org.jwz.demo.Axe;
    
    public class SteelAxe implements Axe {
        @Override
        public String chop() {
            return "使用钢斧砍柴!·";
        }
    }

    ```
5. 定义Person接口实现类`Chinese.java`和`American.java`
    ```java
    package org.jwz.demo.impl;

    import org.jwz.demo.Axe;
    import org.jwz.demo.Person;

    public class Chinese implements Person {
        private Axe axe;
    
        // 用于设值注入的setter方法
        public void setAxe(Axe axe) {
            this.axe = axe;
        }
    
        public Axe getAxe() {
            return axe;
        }
    
        @Override
        public void useAxe() {
            // 调用axe的chop()方法,表明Chinese对象依赖于Axe对象
            // 但具体依赖于StoneAxe还是SteelAxe仍无法确定
            // 需通过Spring配置文件配置其依赖关系
            System.out.println("中国人" + axe.chop());
        }
    }
    ```
    ```java
    package org.jwz.demo.impl;

    import org.jwz.demo.Axe;
    import org.jwz.demo.Person;
    
    public class American implements Person {
        private Axe axe;
    
        // 用于构造注入的带参构造器
        public American(Axe axe) {
            this.axe = axe;
        }
    
        @Override
        public void useAxe() {
            // 调用axe的chop()方法,表明American对象依赖于Axe对象
            // 但具体依赖于StoneAxe还是SteelAxe仍无法确定
            // 需通过Spring配置文件配置其依赖关系
            System.out.println("美国人"+axe.chop());
        }
    }
    ```
    
6. 新建Spring配置文件`src/demo.xml`[名称可以随意指定]
    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xsi:schemaLocation="http://www.springframework.org/schema/beans
           http://www.springframework.org/schema/beans/spring-beans.xsd">
    
        <!-- 配置一个名为"stoneAxe",实现类为"org.jwz.demo.impl.StoneAxe"的Bean -->
        <bean id="stoneAxe" class="org.jwz.demo.impl.StoneAxe"/>
        <bean id="steelAxe" class="org.jwz.demo.impl.SteelAxe"/>
        
        <bean id="chinese" class="org.jwz.demo.impl.Chinese">
            <!--设值注入:驱动Spring容器执行setAxe()方法来注入该Bean的依赖关系-->
            <property name="axe" ref="steelAxe"/>
        </bean>
        <!--<bean />元素默认驱动Spring容器调用该实现类的无参构造器来创建该Bean的实例-->
        <bean id="american" class="org.jwz.demo.impl.American">
            <!--构造注入:驱动Spring容器调用该实现类带参数的构造器来创建该Bean的实例-->
            <constructor-arg ref="stoneAxe" />
        </bean>
    
    </beans>
    ```
7. 新建测试类`Main.java`
    ```java
    package org.jwz;

    import org.jwz.demo.Person;
    import org.springframework.context.ApplicationContext;
    import org.springframework.context.support.ClassPathXmlApplicationContext;
    
    public class Main {
        public static void main(String[] args) {
            // 从类加载路径下加载Spring配置文件并获取Spring容器
            ApplicationContext ctx = new ClassPathXmlApplicationContext("demo.xml");
            // 与传统相比,不再通过new获取对象,而是直接从容器中获取id=chinese的Bean
            Person person = ctx.getBean("american", Person.class);
            // 调用useAxe()方法
            person.useAxe();
        }
    }
    ```
9. 编译运行,程序根据配置文件注入的依赖关系输出相应的结果