# web

Web 服务器

1. 监听一个 TCP 端口
2. 转发请求 (Tomcat 转发给 java 程序)，回收响应
3. 本身没有业务逻辑，只用于连接操作系统和应用程序代码

Servlet

- 一种规范：约束了 Java 的 Web 服务器 (如 Tomcat) 与业务类的通信方式
- 一个接口：javax.servlet.Servlet
- 一种 Java 类：实现了 Servlet 接口的应用程序实现类

类加载器可以通过类全限定名获取类的二进制字节流

解析二进制字节流，获取 Class 类实例

加载 classpath 下的静态资源


Java 类文件规范

统一的 Resource 抽象

每个类文件和类名对应

包命和文件夹路径对应
