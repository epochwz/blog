# Servlet & JSP

## Servlet

Servlet(Server Applet, 服务器小程序), 运行在 Web 服务器中的 Java 小程序，主要用于接收并响应客户端请求，生成动态 Web 内容

### Servlet 生命周期

1. 装载：Tomcat 启动时扫描 web.xml, 解析声明的 Servlet 及其绑定的 URL
2. 创建：第一次访问 Servlet 绑定的 URL 时，Tomcat 才会使用 Servlet 的构造器创建 Servlet 实例
   - 指定了 `load-on-start` 的 Servlet 则会在 Tomcat 启动后进行创建并初始化
3. 初始化：执行 Servlet 的 init 方法
4. 服务：当 Tomcat 运行时，会执行 service doGet doPost 方法来接收和响应请求
5. 销毁：当 Tomcat 停止时，会执行 destroy 方法

*一个 Web 应用程序在运行时，同一个 Servlet 类有且只有一个实例*

### 三大作用域对象

| 对象               | 作用域                          |
| ------------------ | ------------------------------- |
| HttpServletRequest | 同一次请求中                    |
| HttpSession        | 同一次会话中                    |
| ServletContext     | 整个 Web 应用启动到停止的过程中 |

ServletContext 是 Web 应用上下文对象（Web 应用全局对象），一个 Web 应用只会创建一个 ServletContext 对象，随着 Web 应用启动而自动创建

### 请求转发和请求重定向

- 请求转发：浏览器发起请求，服务器接收请求的 Servlet 处理后将请求转发给另一个 Servlet ，直到服务器发起响应，浏览器只发起一次请求
- 请求重定向：浏览器发起请求，服务器接收请求的 Servlet 处理后发起响应，并告知浏览器重新请求新的 URL，至此浏览器发起两次请求

## JSP

Servlet 有如下缺点

- 静态 HTML 和动态 Java 代码混合在一起，难以维护
- Servlet 使用 out.println 输出页面代码，效率低下
- 开发工具难以检测页面代码中的错误，调试困难（页面代码以字符串形式硬编码在 Java 代码中）

JSP(Java Server Page,Java 服务器页面)，用于降低动态网页的开发难度，由 Web 服务器执行

- 将 Java 代码与 HTML 代码分离，使用简单，降低开发难度，易于维护
- JSP 的本质还是 Servlet

### 语法规则

- JSP 注释：`<%-- --%>`
- JSP 代码块：在 JSP 中嵌入 Java 代码块 `<% JAVA CODE BLOCK %>`
- JSP 声明构造块：在 JSP 中声明变量或方法 `<%! DECLARATION STATEMENT %>`
- JSP 输出指令：在 JSP 页面中显示 Java 代码执行结果 `<%= String %>` 相当于 `out.println()`
- JSP 处理指令：提供 JSP 执行过程中的辅助信息 `<%@ JSP instruction %>`
  - `<%@ page %>` 定义当前 JSP 页面的全局设置
  - `<%@ include %>` 合并其他 JSP 页面
  - `<%@ taglib %>` 引入 JSP 标签库

### 九大内置对象

JSP 内置对象就是无需显式声明的、可以直接在 `<% Java Code Block %>` 中使用的 Servlet 对象

| JSP 对象    | Servlet 对象        | 功能描述     |
| ----------- | ------------------- | ------------ |
| request     | HttpServletRequest  | 请求对象     |
| response    | HttpServletResponse | 响应对象     |
| session     | HttpSession         | 用户会话     |
| application | ServletContext      | 应用上下文   |
| pageContext | PageContext         | 页面上下文   |
| page        | this                | 当前页面对象 |
| out         | PrintWriter         | 输出对象     |
| config      | ServletConfig       | 应用配置对象 |
| exception   | Throwable           | 应用异常对象 |

其中，`exception` 对象只能在指定了 `isErrorPage="true"` 的 JSP 页面中才能够使用

```jsp
<%@ page contentType="text/html;charset=UTF-8" isErrorPage="true" %>
```

## 中文乱码

Tomcat 默认使用西欧字符集 `ISO-8859-1`，因此解决 Servlet 开发中出现的中文乱码的核心思路是将 `ISO-8859-1` 转换成 `UTF-8`

1. JSP 页面需要设置使用 `UTF-8` 字符集
2. Servlet 接收请求时需要指定使用 `UTF-8` 字符集进行处理
   - *POST*
     - 添加 `request.setCharacterEncoding("UTF-8");`, 将 *请求体* 中的内容转换成 `UTF-8` 字符集
     - *注意*：由于 `GET` 请求通常将参数追加在 URL 上，而不是通过请求体传参，因此该方法无法处理 `GET` 请求
   - *GET*
     - Tomcat8.x 以上，对于 Get 请求中的参数会自动转换成 `UTF-8` 字符集，因此无需额外处理
     - Tomcat8.x 以下，需要在 Tomcat 配置文件 `server.xml` 中添加 `URIEncoding="UTF-8"`
3. Servlet 发送响应时需要指定使用 `UTF-8` 字符集进行处理
   - 获取页面输出流 `response.getWriter()` 之前，先设定响应内容所使用的字符集 `response.setContentType("text/html;charset=UTF-8");`
