# JavaEE

**JavaEE(Java Platform Enterprise Edition)** 是 `Java 平台企业级应用开发` 的一整套技术规范和指南，包含 13 个功能模块，核心功能是 Web 应用程序开发，其具体实现由软件厂商（如 Apache）提供

| 模块名称 | 模块功能          |
| -------- | ----------------- |
| Servlet  | Web 服务器小程序  |
| JSP      | 服务器页面        |
| JDBC     | 数据库交互模块    |
| XML      | XML 交互模块      |
| EJB      | 企业级 Java Bean  |
| RMI      | 远程调用          |
| JNDI     | 目录服务          |
| JMS      | 消息服务          |
| JTA      | 事务管理          |
| JavaMail | 发送 / 接收 Email |
| JAF      | 安全框架          |
| CORBA    | CORBA 集成        |
| JTS      | CORBA 事务监控    |

## JavaWeb

**Java Web** 是 JavaEE 的一部分，主要是指 JavaEE 中 Servlet、JSP 等 Web 应用程序开发的核心模块

## Tomcat

Tomcat 是 `Apache Software Foundation` 旗下一款免费开源的 Web 应用服务器程序，是 JavaWeb 标准 (Servlet & JSP) 的实现者

Tomcat 的运行离不开 JavaSE, JavaSE 是 JavaEE 的基石

**基于 Tomcat 的 JavaWeb 应用的标准目录结构**

```bash
CATALINA_HOME/webapps           Tomcat 应用根目录
    WebProjectName              JavaWeb 应用根目录
        index.html              默认首页
        WEB-INF                 安全目录，用于存放配置文件
            web.xml             核心配置文件，又称“部署描述符文件”
            classes             Java 类编译后的字节码文件
            lib                 JavaWeb 应用依赖的 JAR 包
        META-INF/MANIFEST.MF    JavaWeb 应用的辅助描述信息，如应用版本信息等
```
