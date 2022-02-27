Filter是Servlet的加强版，是典型的处理链:
1. 对用户请求进行预处理:在request到达Servlet之前,拦截客户的request,进行检查/修改request头和数据
2. 将请求交给Servlet进行处理并生成响应
3. 对服务器响应进行后处理:在response到达客户端之前拦截response,根据需要进行检查/修改response头和数据

Filter实际上是从多个Servlet抽取出来的通用代码，执行一些对用户请求的通用处理[权限控制、日志记录],分类如下:
1. 用户授权Filter:负责检查用户请求，根据请求过滤用户非法请求
2. 日志Filter:详细记录某些特殊的用户请求
3. 负责解码的Fitler:包括对非标准编码的请求解码
4. 能改变XML内容的XSLT Filter等

Filter可负责拦截多个请求和响应;一个请求或响应可以被多个Filter拦截

创建Filter的步骤
1. 创建Filter处理类,实现javax.servlet.Filter接口
2. 配置Filter
    1. 注解配置
        ```
        @WebFilter(
            <!--1. 配置Filter名-->
            filterName="filterName"
            <!--2. 配置Fitler拦截的URL,可使用模式字符串拦截多个请求-->
            urlPatterns/value="/url"
            <!--配置Filter参数-->
            initParams
            <!--指定该Filter仅过滤该属性指定多个Servlet的名称-->
            servletNames
            <!--指定Filter仅对该属性指定的dispatcher模式的请求,默认全部过滤-->
            dispatcherTypes=ASYNC/ERROR/FORWARD/INCLUDE/REQUEST
            <!--Filter的显示名-->
            displayName
            <!--Filter是否支持异步操作模式-->
            asyncSupported
        )
        ```
    2. web.xml配置
        ```
        <filter>
            <!--Filter名称-->
            <filter-name>FilterName</filter-name>
            <!--Fitler实现类-->
            <filter-class>package.FilterClass</filter-class>
            <!--配置Filter的参数-->
            <init-param>
                <param-name>paramName</param-name>
                <param-value>paramValue</param-value>
            </init-param>
        </filter>
        <filter-mapping>
            <filter-name>FilterName</filter-name>
            <!--Filter拦截的URL-->
            <url-pattern>/*</url-pattern>
        </filter-mapping>
        ```

# 使用URL Rewrite实现网站伪静态
搜索引擎会优先收录静态页面，可以通过Filter拦截所有发向*.html请求，然后按照某种规则将请求forward到实际的*.jsp页面即可
1. 添加UrlRewrite的JAR包
2. web.xml配置UrlRewrite Filter
    ```xml
    <!-- 配置网站伪静态Filter -->
    <filter>
        <filter-name>UrlRewriteFilter</filter-name>
        <filter-class>org.tuckey.web.filters.urlrewrite.UrlRewriteFilter</filter-class>
    </filter>
    <filter-mapping>
        <filter-name>UrlRewriteFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
    ```

3. 定义伪静态映射规则:WEB-INF/urlrewrite.xml
    ```xml
    <?xml version="1.0" encoding="GBK"?>
    <!DOCTYPE urlrewrite PUBLIC "-//tuckey.org//DTD UrlRewrite 3.2//EN"
    	"http://tuckey.org/res/dtds/urlrewrite3.2.dtd">
    <urlrewrite>
    	<rule>
    		<!-- 所有匹配如下正则表达式的请求 -->
    		<from>/userinf-(\w*).html</from>
            <!-- 被forward到如下JSP页面,$1代表第一个匹配正则的字符串 -->
    		<to type="forward">/userinf.jsp?username=$1</to>
    	</rule>
    </urlrewrite>
    ```


# Listener
Listener:监听Web应用的内部事件,当Web内部事件发生时回调事件监听器内的方法
1. 配置Listener
    - 注解配置:`@WebListener`
    - web.xml配置
        ```
        <listener>
            <listener-class>package.ListenerClass</listener-class>
        </listener>
        ```
2. 定义Listener实现类,继承相应的Listener接口
    1. ServletContextListener:用于监听Web应用的启动和关闭,启动后台程序支持系统运行
        - 本身不能配置初始化参数，只能获取应用的配置参数
        - 相当于load-on-startup Servlet
    2. ServServletContextAttributeListener:用于监听application范围内属性的变化
    3. ServletRequestListener:用于监听用户请求的初始化和销毁,实现系统日志
    4. ServletRequestAttributeListener:用于监听request范围属性的改变
    5. HttpSessionListener:用于监听用户session的开始和结束，监听系统的在线用户
    6. HttpSessionAttributeListener:用于监听session范围内属性的改变