# 过滤器 & 监听器

## 过滤器

作用：实现对 Web 资源请求的拦截，完成特殊的操作（预处理和后处理）

- Web 资源：JSP、Servlet、图片、文件

应用场景

- Web 资源权限访问控制
- 请求字符集编码处理
- 内容敏感字符词汇过滤
- 响应信息压缩处理

**工作流程**

![过滤器工作流程](../../resources/images/javaweb/filter-workflow.png)

**生命周期**

1. Web 应用启动时，Web 服务器创建并初始化 Filter 实例
2. 当客户端请求过滤器相关联的资源时，过滤器 拦截请求并进行相应的预处理
3. Filter 实例创建后会一直驻留在内存中，直到 Web 应用对其进行移除，或者 Web 服务器停止时才被销毁
4. 过滤器的创建和销毁都是由Web服务器负责的，Web 应用只需要关注过滤器的预处理和后处理

**开发步骤**

1. 实现 Filter 接口，实现 doFilter 方法
2. 在 web.xml 中注册 Filter 实现类，设置拦截的资源

**过滤器链**

- 在一个 Web 应用中，多个 Filter 组合起来称为过滤器链
- 过滤器的调用顺序取决于过滤器在 web.xml 中的注册顺序：预处理是顺序，后处理是逆序

## 监听器

**定义**：Servlet 规范定义的一种特殊类，用于监听 Web 应用组件 的创建、销毁、属性变化等事件，并在事件发生前后进行一些必要的处理

- 监听人：Web 应用服务器
- 监听器
- 监听对象：ServletContext、HttpSession、ServletRequest

应用场景

- 统计在线人数
- 统计页面访问量
- 应用启动时完成信息初始化工作
- 整合 Spring

**开发步骤**

1. 实现监听器接口
2. 在 web.xml 中注册 Listener 实现类

监听器的调用顺序取决于监听器在 web.xml 中的注册顺序：初始化是顺序，销毁是逆序

**监听器的分类**

按监听对象分类

- 容器监听器 ServletContextListener
  - Web 应用启动时完成 ServletContext 的创建和初始化，在 Web 应用被移除、关闭的时候完成 ServletContext 的销毁
  - 应用：在 web.xml 中配置初始化参数（数据库连接、系统名称、版本号）
- 会话监听器
- 请求监听器

按监听事件分类

- 域对象自身的创建和销毁
- 域对象中属性的创建、替换、消除
- 绑定到 session中的某个对象的状态事件 HttpSessionBindingListener
  - 当监听器对象绑定至HTTP会话时调用
  - 当监听器对象从HTTP会话中移除、修改、或会话销毁时调用
  - 无需注册监听器


```xml
<filter-mapping>
    <filter-name>Charset</filter-name>
    <url-pattern>/*</url-pattern>
    <!-- 指定拦截特定类型的请求，默认是 REQUEST -->
    <dispatcher>REQUEST</dispatcher>
    <!--<dispatcher>FORWARD</dispatcher>-->
    <!--<dispatcher>INCLUDE</dispatcher>-->
    <!--<dispatcher>ERROR</dispatcher>-->
</filter-mapping>
```