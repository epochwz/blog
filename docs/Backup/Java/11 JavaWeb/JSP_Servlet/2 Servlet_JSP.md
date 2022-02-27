# Servlet简介
JSP只负责简单的逻辑显示，无法直接访问应用的底层状态，因此JavaEE应用使用JavaBean来传输数据[封装应用底层的状态信息]

JSP充当表现层技术，在底层被Web服务器编译成Servlet，真正运行在web服务器的是Servlet

Servlet:继承了HttpServlet类的特殊Java类，作为控制器运行在服务器端,负责处理及响应客户端请求，通常称为服务器端小程序

HttpServlet提供的方法
1. doGet用于响应GET请求
2. doPost用于响应POST请求
3. doPut用于响应PUT请求
4. doDelete用于响应DELETE请求
5. service响应所有请求
6. init(ServletConfig config):创建Servlet实例时调用该方法初始化Servlet资源
    - 通常无须重写，除非需要在初始化Servlet时完成某些资源的初始化;
    - 若重写则需要在第一行通过super.init(config)调用HttpServlet的init方法
7. destroy():销毁Servlet实例时调用该方法回收资源
    - 通常无须重写，除非进行关闭资源等操作

# Servlet的生命周期
JSP/Servlet/Filter/Listener都必须运行在Web容器中，由Web服务器调用

每个Servlet类只有一个实例,在运行时的创建和销毁是由Web容器控制的,其生命周期如下:
1. 创建Servlet实例
    1. 当客户端第一次请求某个Servlet时，系统创建其实例
    2. Web应用启动时立即创建Servlet实例，即load-on-startup Servlet
2. Web容器调用Servlet的init方法初始化Servlet实例
3. Servlet初始化后将一直存在于容器中，根据每一次用户请求动态生成标准HTML页面输送到客户端
4. Web容器决定销毁Servlet时[通常是在关闭Web应用时]，先调用其destroy()方法回收资源

# load-on-startup
后台Servlet常作为应用的基础Servlet使用，不能接收用户请求，只能在应用启动时实例化;无须响应用户请求，不需要service方法
1. web.xml中配置<servlet>的子元素<load-on-startup>1</load-on-startup>
2. 注解配置:@WebServlet(loadOnStartup=1)整数值越小越优先实例化

# JSP的基本原理
Servlet利用输出流动态生成HTML页面[HTML标签+内容],开发效率极低。 

JSP是Servlet类的简化,第一次收到请求时必须先编译成Servlet类
- JSP页面=静态部分[HTML标签+静态内容]+动态部分[Java脚本控制的动态内容]
- JSP的脚本内容和静态内容转换成Servlet的_jspService的执行代码，JSP声明转换成Servlet的成员变量和成员方法
- 因此JSP页面的第一次访问速度比较慢
- JSP、Servlet之间不会相互调用，而是通过4个Map结构来共享数据
    - page本页面有效
    - request本次请求有效
    - session本次用户会话有效
    - application本应用内一直有效

# JSP的4种基本语法
```
<%@ page contentType="text/html;charset=utf-8" %>
<!DOCTYPE html>
<html>
    <head>
        <title>Demo_JSP</title>
    </head>
    <body>
        <!-- HTML注释:会发送到客户端，可以从源代码中查看相关信息 -->
        <!-- 1. JSP注释:不会输出到客户端，无法从源代码中查看相关信息 -->
        <%--  --%>

        <!-- 2. JSP声明:用于声明变量和方法，会转换成对应Servlet的成员变量或成员方法 -->
        <!--    JSP声明不能使用abstract修饰方法，否则将导致生成的Servlet变成抽象类，无法实例化 -->

        <%!  %>
        <!-- 3. JSP输出表达式:表达式后不能有分号 -->
        <%= %>

        <!-- 4. JSP脚本:控制页面的静态内容 -->
        <!-- JSP脚本可以嵌入任何可执行性代码;可以声明变量,相当于Servlet类_jspService的局部变量 -->
        <%  %>
    </body>
</html>
```

# JSP的3个编译指令
编译指令的语法格式:`<%@ 编译指令名 属性名="属性值" %>`
1. page指令`<%@ page name="value" %>`:针对当前页面的指令
    ```
    language="Java":声明当前JSP页面使用的脚本语言，默认java，无须设置
    contentType="text/html;charset="ISO-8859-1":设定生成网页的文件格式[MIME类型]和编码字符集[页面字符集]
    pageEncoding="ISO-8859-1":指定生成网页的编码字符集
    import:导入JAR包，默认导入java.lang.*;javax.serlet.*;javax.servlet.jsp.*;javax.servlet.http.*;
    extends:指定JSP页面编译所产生的Servlet类所继承的父类/接口
    errorPage:指定错误处理页面，产生异常时自动调用该属性所指定的JSP页面,否则系统将直接把异常信息呈现给客户端浏览器
    isErrorPage="true":设置本页面为错误处理程序,无须指定errorPage属性
    info:设置该JSP程序的信息，通过this/page.getServletInfo()方法获取
    session:设定该页面是否需要HTTPSession
    isThreadSafe:是否线程安全
    buffer=none|8kb|x kb:指定输出缓冲区的大小，用于缓存JSP页面对客户端浏览器的输出
    autoFlush:当缓冲区溢出时是否强制输出缓冲区内容，设置为false则溢出时产生一个异常
    ```
2. 静态include指令`<%@ include file="filePath" %>`
    - 将目标页面的其它编译指令包含进来,解析该页面中的JSP语句,"融合"成一个页面[Servlet类]
3. taglib指令`<%@ taglib uri="" prefix="" %>`:用于定义和访问自定义标签

# JSP的7个动作指令
编译指令通知Servlet引擎处理消息，在将JSP编译成Servlet时起作用[编译指令相当于脚本的标准化写法,通常可替换成JSP脚本],而动作指令只是运动时的动作
```
<!-- 1. 将用户请求"转发"到指定页面:实际上并没有向新页面发送请求,只是完全以新页面来响应请求,因此用户的请求地址不变，请求参数不会丢失 -->
<jsp:forward page="*.html|*.jsp|servlet类" >
    <!-- 2. 用于传递参数，必须配合支持参数的标签使用[include/forward/plugin] -->
    <!--    通过HttpServletRequest类的getParameter(paramName)方法获取 -->
    <jsp:param name="参数名" value="参数值" />
</jsp:forward>

<!-- 3. 动态导入指定的页面，不会导入其编译指令，仅将其body内容插入本页面，可以带参数 -->
<jsp:include page="" flush="是否将缓存转移到导入的页面">

<!-- 4. 创建一个JavaBean实例 -->
<jsp:useBean id="name" class="classname" scope="" />
<!-- JSP中设置/获取属性实际上是调用Servlet中的getter/setter,与Servlet类是否定义了该属性无关 -->
<!-- 5. 设置JavaBean实例的属性值 -->
<jsp:setProperty name="BeanName" proteryt="propertyName" value="value" />
<!-- 6. 输出JavaBean实例的属性值 -->
<jsp:setProperty name="BeanName" proteryt="propertyName" />
<!-- 7. 下载JavaBean或Applet到客户端执行 -->
<jsp:plugin ></jsp:plugin>
```
==注意==:forward拿目标页面代替原有页面，静态include融合目标页面和原有页面成一个Servlet，动态include在Servlet中调用了include方法将目标页面的body内容插入原有页面

# JSP/Servlet的9个对象
JSP脚本中的9个内置对象都是Servlet API接口的实例，JSP页面对应的Servlet的_jspService()方法默认创建了这些实例[形参、局部变量]
- 因此可以直接在JSP脚本、输出表达式中使用这些对象，声明中则不能使用;
- 只有异常处理页面对应的Servlet才会初始化exception对象

## application对象
application==javax.servlet.ServletContext:代表Web应用本身，每个Web应用只有一个ServletContext实例
1. 在整个Web应用中的JSP/Servlet之间共享数据
    - `setAttribute(paramName,paramValue)/getAttribute(paramName,paramValue)`
    - 通常只将Web应用的**状态数据**放入application中，而不要仅仅为了共享数据
2. 获得Web应用配置参数`getInitParameter(paramName)`
    - Web应用配置参数对整个Web应用有效:将配置信息放在web.xml文件中，避免使用硬编码写在代码中

## config对象
config==javax.servlet.ServletConfig:代表JSP/Servlet的配置信息
- 在web.xml中配置JSP[当成Servlet]，可以为其指定配置信息，映射成指定的URL
    - 其配置信息必须通过该URL才能访问,否则相当于普通JSP，没有所谓的配置信息
- config对象获取JSP/Servlet的配置参数`config.getInitParameter(paramName)`

## exception对象
exception==java.lang.Throwable:代表其它页面的异常和错误。
- 当isErrorPage=true时才可使用
- JSP的异常处理机制只对JSP脚本和输出表达式起作用，JSP声明仍然需要处理异常

## out对象
out==javax.servlet.jsp.JspWriter:代表JSP页面的输出流，用于输出内容形成HTML页面

所有使用输出表达式的地方都可以使用out.println()代替;输出表达式的本质是out.write()

## page对象
page==javax.servlet.jsp.PageContext:代表JSP页面本身，相当于Servlet中的this，其类型就是生成的Servlet类

## pageContext对象
pageContext==javax.servlet.jsp.PageContext:代表JSP页面上下文，用于访问JSP页面中的共享数据
- getAttribute(String name):获取page范围的name属性
- getAttribute(String name,int scope):获取score指定范围的name属性
    - score=pageContext.PAGE_SCOPE|REQUEST_SCORE|SESSION_SCOPE|APPLICATION_SCOPE
- setAttribute(String name,String value):设置page范围的name属性
- setAttribute(String name,String value,int scope):设置score指定范围的name属性
- 
- getAttributeScope(paramName)获取属性所在范围
- 
- getServletContext()获取ServletContext对象
- getServletConfig()获取ServletConfig对象
- getRequest()获取ServletRequest对象
- getResponse()获取ServletResponse对象
- getSession()获取HttpSession对象

## request对象
request==javax.servlet.http.HttpServletRequest，该对象封装了一次用户请求，代表本次请求范围，可操作request范围的属性

Web应用基于请求/响应架构，浏览器发送请求时附带一些请求头/参数发送给服务器，JSP/Servlet通过request获取请求头/请求参数,服务器通过JSP/Servlet解析请求头/请求参数,
1. 获取请求头
    - String getHeader(String name)获取指定请求头的值
    - java.util.Enumeration<String> getHeaderNames()获取所有请求头的名称
    - java.util.Enumeration<String> getHeaders(String name)获取指定请求头的多个值
    - int getIntHeader(String name)获取指定请求头的值并转换为整数值
2. 获取请求参数
    - String getParameter(String paramName)获取请求参数的值
    - Map getParameterMap()获取所有请求参数的name-value对组成的Map对象
    - Enumeration getParameterNames()获取所有请求参数名组成的Enumeraton对象
    - String[] getParameterValues(String name)获取请求参数的值，当该参数有多个值时返回组成的数组
3. 操作request范围的属性
    - setAttribute(String attName,Object attValue)
    - Object getAttribute(String attName)
4. 执行forward或include
    - RequestDispatcher rd=request.getRequestDispatcher("/url")
    - rd.forward(request,response)
    - rd.include(request,response)

## response对象
response==javx.servlet.http.HttpServletResponse，代表服务器对客户端的响应
- 通常用于响应非字符内容/重定向/向客户端增加Cookie等
- out==JspWriter,JspWriter是Writer的子类，字符流，无法输出非字符内容

1. 生成非字符响应[图片、PDF、Excel]
    - response.getOutputStream()方法返回字节输出流
2. 重定向:生成第二次请求，丢失所有请求参数和request范围属性
    - response.sendRedirect(String path):重定向到path资源，即重新向path发送请求
3. 增加Cookie:用于网站记录客户的某些信息，会一直存放在客户端机器上，直到超出其生命期限
    ```
    Cookie c=new Cookie(String name,String value):创建Cookie对象
    c.setMaxAge(24*3600):设置Cookie的生存期限，默认随浏览器关闭而消失
    response.addCookie(c):写入Cookie
    Cookie[] cs=request.getCookies():获取客户端所有Cookie
    cs[i].getName();cs[i].getValue();
    ```
    - 中文Cookie:java.net.URLEncoder.encode(value,charset)先对中文字符串编码，结果设置为Cookie值，读取Cookie后java.net.URLDecoder.decode(value)进行解码

## session对象
session==javax.servlet.http.HttpSession:代表一次用户会话[浏览器打开并连接服务器到断开服务器并关闭]
- session用于跟踪用户会话信息，用于判断是否登陆,购物车应用等，通常只应该将用户会话状态相关的信息放入session范围中
- session用于保存客户端状态信息，这些信息需要保存到Web服务器的硬盘上，要求session的属性值是可序列化的，否则引发不可序列化异常