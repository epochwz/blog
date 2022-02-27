# 构建Servlet应用
1. 添加`Tomcat_HOME/lib`下的`servlet-api.jar、jsp-api.jar`到类加载路径
2. 新建Servlet源文件`ServletDemo.java`,将编译后的.class文件部署在`WEB-INF/classes/`路径下
    ```java
    package jwz;

    import javax.servlet.*;
    import javax.servlet.http.*;
    import javax.servlet.annotation.*;
    
    // 使用注解方式再Web应用中部署Servlet
    @WebServlet(
        name="ServletDemo",
        value="/ServletDemo"
    )
    public class ServletDemo extends HttpServlet{
        // Servlet的初始化方法
        public void init(ServletConfig config) throws ServletException{
            super.init(config);
        }
    
        // 客户端的响应方法，使用该方法可以响应客户端所有类型的请求
        public void service(HttpServletRequest request,HttpServletResponse response) throws ServletException{
            System.out.println(request.getParameter("username"));      
        }
    }
    ```
3. 访问`http://localhost:8080/WebDemo/ServletDemo?username=jwz`，控制台输出jwz

# 在Web应用中部署Servlet的两种方式
1. 配置web.xml
    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee
        http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
        version="3.1">
        
        <!-- 配置Web应用首页列表 -->
        <welcome-file-list>
            <welcome-file>index.html</welcome-file>
            <welcome-file>index.htm</welcome-file>
            <welcome-file>index.jsp</welcome-file>
        </welcome-file-list>
        
        <!-- 配置Web应用的参数 -->
        <context-param>
            <param-name>paramName</param-name>
            <param-value>paramValue</param-value>
        </context-param>
        
        <!-- 配置JSP/Servlet -->
        <servlet>
            <!-- 配置Servlet名称 -->
            <servlet-name>servletName</servlet-name>
            <!-- 1. 配置JSP -->
            <jsp-file>/jspName.jsp</jsp-file>
            <!-- 2. 配置Servlet -->
            <servlet-calss>package.className</servlet-calss>
            <!-- 配置Servlet参数 -->
            <init-param>
                <param-name>paramName</param-name>
                <param-value>paramValue</param-value>
            </init-param>
        </servlet>
        <!-- 配置Servlet映射路径 -->
        <servlet-mapping>
            <!-- 配置进行映射的Servlet名称 -->
            <servlet-name>servletName</servlet-name>
            <!-- 配置映射成的URL路径 -->
            <url-pattern>/servletPath</url-pattern>
        </servlet-mapping>
        
    </web-app>
    ```
2. 使用@WebServlet注解进行配置:
    - web.xml中不要配置该Servlet
    - web.xml中<web-app>元素的metadata-complete="false"或不设置
    ```
    @WebServlet(
        // Servlet的名称
        name="servletName",
        // Servlet拦截的url
        urlPatterns/value="/servletPath",
        // 将该Servlet配置成随服务器启动的Servlet,值越小越优先实例化
        loadOnStartup=1,
        // 配置Servlet初始化参数
        initParams={
            @WebInitParam(name="driver",value="com.mysql.jdbc.Driver"),
            @WebInitParam(name="url",value="jdbc:msyql://localhost:3306/javaee"),
            @WebInitParam(name="user",value="root"),
            @WebInitParam(name="pass",value="toor")
        },
        // 指定Servlet的显示名
        displayName="",
        // 指定该Servlet是否支持异步操作模式
        asyncSupported=true
    )
    ```