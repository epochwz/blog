# Bean的别名
<bean />元素的id属性是真正的XML ID属性,必须由字母开头,只能由数字和字母组成
- 使用name属性在定义Bean时指定Bean的别名`<bean id="person" class="..." name ="#abc,@*123,sdf" />`
- alias为已经存在的Bean实例指定别名`<alias name="person" alias="jack" />`

# Bean的作用域
作用域 | 描述
---|---
singleton | 单例模式,整个IoC容器中只生成一个实例
prototype | 每次通过getBean()方法获取Bean时都将产生一个新的实例
request | 在一次HTTP请求内,程序每次请求该Bean得到的总是同一个实例
session | 在一次HTTP会话内,session作用域的Bean只生成一个Bean实例
global session | 每个全局HTTP Session对应一个Bean实例,仅在使用portlet context时有效

singleton:Bean的默认作用域,容器负责跟踪Bean实例的状态,维护其生命周期行为
- `<bean id="" class="" />`

prototype:容器仅仅负责创建Bean实例,并返回给程序,然后不在跟踪维护该实例
- `<bean id="" class="" scope="prototype" />`

request/session/global session:只在Web应用中生效,一般只将Web应用的控制器Bean设置成request作用域
- 必须在`Web.xml`中增加额外配置,将HTTP请求对象绑定到为该请求提供服务的线程上,使该作用域的Bean实例能够在后面的调用链中被访问到。
    ```
    <listener>
        <listener-class>
            org.springframework.we.context.request.RequestContextListener
        </listener-class>
    </listener>
    ```
