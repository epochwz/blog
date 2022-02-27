# Spring IoC容器的特点
- 应用程序面向接口编程,将各组件之间的耦合关系提升到接口层次,有利于项目后期扩展
- 应用程序各组件不再由程序主动创建,而是Ioc容器负责产生并初始化
- 调用者需要调用被依赖对象的方法时,等待Spring容器的注入即可,无需主动获取被依赖对象

# 代表Spring IoC容器的接口
1. BeanFactory是Spring容器最基本的接口
    - 常用实现类是DefaultListableBeanFactory
        ```java
        // Resource接口是Spring提供的资源访问接口,可以以简单、透明的方式访问磁盘、类路径以及网络上的资源
        // 1. 查找类加载路径下的beans.xml文件创建Resource对象
        Resource isr=new ClassPathResource("beans.xml");
        // 1. 查找文件系统的当前路径下的beans.xml文件创建Resource对象
        Resource isr=new FileSystemResource("beans.xml");
        // 2. 创建默认的BeanFactory容器
        DefaultListableBeanFactory beanFactory=new DefaultListableBeanFactory();
        // 3. 让默认BeanFactory容器加载isr对应的XML配置文件
        new XmlBeanDefinitionReader(beanFactory).loadBeanDefinitions(isr);
        ```
    - BeanFactory容器初始化后不会立即创建Bean实例,而是等待需要的时候才创建Bean实例
2. ApplicationContext是BeanFactory的子接口,称为Spring上下文
    - 在独立应用程序中使用Spring容器,常用实现类有
        - FileSystemXmlApplicationContext
        - ClassPathXmlApplicationContext
        - AnnotationConfigApplicationContext
            ```java
            // 1. 从类加载路径下加载配置文件,并根据配置文件创建Spring容器
            ApplicationContext ctx=new ClassPathXmlApplicationContext("beans.xml","service.xml");
            // 2. 从文件系统的相对/绝对路径下加载配置文件,并根据配置文件创建Spring容器
            ApplicationContext ctx=new FileSystemXmlApplicationContext("beans.xml");
            ```
    - 在Web容器中使用Spring容器,常用实现类有
        - XmlWebApplicationContext
        - AnnotationConfigWebApplicationContext

# ApplicationContext的额外功能
1. 同时加载多个XML配置文件
2. 资源访问,如URL和文件
3. ApplicationContext继承了MessageSource接口,提供国际化支持
4. ApplicationContext支持事件机制
5. ApplicationContext在Web应用中允许以声明式方式启动并创建Spring容器
    
    在`web.xml`中配置Listener即可
    ```xml
    <listener>
        <!--Web应用启动时自动初始化ApplicationContext实例,前端MVC框架直接调用容器中的Bean即可,无需访问Spring容器本身-->
		<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
	</listener>
    ```
6. ApplicationContext容器初始化完成后，默认会预初始化所有singleton Bean及其依赖的Bean
    - 系统前期创建ApplicationContext实例时有较大开销,但后期程序获取singleton Bean实例时有较好性能,且能在容器初始化阶段检验出配置错误
    - 可通过配置取消预初始化

# ApplicationContext的国际化支持
当程序创建ApplicationContext容器时,Spring自动查找配置文件中名为messageSource的Bean实例,将MessageSource接口定义的国际化方法getMessage()的调用委托给该messageSource Bean
- 若不存在则查找父容器,若再不存在则创建一个空的StaticMessageSource Bean

国际化步骤
1. 基本资源文件`message.properties`
    ```
    now=现在时间是:{0}
    ```
2. 英文资源文件`message_en_US.properties`
    ```
    now=now is {0}
    ```
3. 中文资源文件`message_zh_CN.properties`
    - 通过cmd命令`native2ascii message.properties message_zh_CN.properties`生成
4. 在程序中使用资源文件的key输出国际化消息
    ```java
    // getMessage()方法获取本地化消息
    // Locale.getDefaule()返回计算机环境的默认Locale
    String now = ctx.getMessage("now", new Object[]{new Date()},Locale.getDefault(Locale.Category.FORMAT));
    ```
# ApplicationContext的事件机制----观察者模式
![Spring的事件机制](http://note.youdao.com/yws/public/resource/90f5a93f88a72c62f0e6f058d17687b1/xmlnote/921BE4715AF2452489F9B8E91813440B/15345)

事件机制开发步骤
1. 继承ApplicationEvent定义Spring容器事件`MyEvent.java`
    ```java
    package org.jwz.event;

    import org.springframework.context.ApplicationEvent;
    
    /**
     * 自定义容器事件
     */
    public class MyEvent extends ApplicationEvent {
        // 事件内容
        private String event;
    
        public MyEvent(Object source, String event) {
            super(source);
            this.event = event;
        }
        
        public String getEvent() {
            return event;
        }
    
        public void setEvent(String event) {
            this.event = event;
        }
    }

    ```
2. 实现ApplicationListener接口定义Spring容器事件监听器`MyListener.java`
    ```java
    package org.jwz.event;

    import org.springframework.context.ApplicationEvent;
    import org.springframework.context.ApplicationListener;
    
    /**
     * 自定义事件监听器
     */
    public class MyListener implements ApplicationListener{
    
        // 当ApplicationContext发布事件时自动触发该监听器
        @Override
        public void onApplicationEvent(ApplicationEvent applicationEvent) {
            // 监听程序触发的事件[MyEvent]
            if (applicationEvent instanceof MyEvent) {
                System.out.println("自定义容器事件:"+((MyEvent) applicationEvent).getEvent());
            }
            // 监听容器内置事件:可以自定义Spring容器初始化、销毁时的回调方法
            else{
                System.out.println("Spring容器内置事件:"+applicationEvent);
            }
        }
    }
    ```
3. 配置事件监听器`<bean class="package.MyListener" />`
4. 发布事件`ctx.publishEvent(new MyEvent("MyEvent","你的计算机即将爆炸!"));`
