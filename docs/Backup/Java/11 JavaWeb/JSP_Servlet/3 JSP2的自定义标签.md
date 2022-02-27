JSP自定义标签:类似HTML标签的语法,封装JSP脚本的功能,取代丑陋的JSP脚本
- JSP脚本和HTML5混杂，难以阅读，维护成本高，美工人员无法参与开发

# 开发自定义标签库的步骤
1. 开发自定义标签处理类:在底层提供JSP页面标签的支持
    ```java
    package jwz;

    import javax.servlet.jsp.tagext.*;
    import javax.servlet.jsp.*;
    import java.io.*;
    
    // 1. 继承javax.servlet.jsp.tagext.SimpleTagSupport
    public class TagDemo extends SimpleTagSupport{
        // 2. 若标签类包含属性,则必须有对应的getter/setter方法
        // 3. 重写doTag()方法，负责生成页面内容
        public void doTag() throws JspException,IOException{
            // 获取页面输出流，并输出字符串
            getJspContext().getOut().write("恭喜你,自定义标签成功:" + new java.util.Date());
        }
    }
    ```
2. 建立一个*.tld文件，放在Web应用的WEB-INF路径或子路径下，Web容器会自动加载该文件
    - TLD文件[Tag Library Definition]称为标签库定义文件,对应一个标签库,可以包含多个标签
    ```xml
    <?xml version="1.0" encoding="utf-8"?>

    <taglib xmlns="http://xmlns.jcp.org/xml/ns/javaee"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee 
        http://xmlns.jcp.org/xml/ns/javaee/web-jsptaglibrary_2_1.xsd"
        version="2.1">
        <!-- 标签库的内部版本号 -->
        <tlib-version>1.0</tlib-version>
        <!-- 标签库的默认短名 -->
        <short-name>mytaglib</short-name>
        <!-- 标签库的唯一标识URI,JSP页面根据该URI定位标签库 -->
        <uri>http://www.jwz.com/mytaglib</uri>
    
        <!-- 定义一个标签 -->
        <tag>
            <!-- 定义标签名 -->
            <name>tagDemo</name>
            <!-- 定义标签处理类 -->
            <tag-class>jwz.TagDemo</tag-class>
            <!-- 定义标签体内容 -->
                <!--tagdependent:标签处理类自己负责处理标签体-->
                <!--empty:该标签只能作为空标签使用-->
                <!--scrpitless:该标签的标签体可以是HTML元素、表达式语言,不允许出现JSP脚本-->
                <!--JSP:标签体可以使用JSP脚本,JSP2规范后不再支持该属性值-->
            <body-content>empty</body-content>
        </tag>
        
    </taglib>
    ```
3. 在JSP文件中使用自定义标签
    1. 导入uri指定的标签库，负责处理mytag前缀的标签:`<%@ taglib uri="http://www.jwz.com/mytaglib" prefix="mytag" %>`
    2. 使用自定义标签:`<mytag: tagDemo />`

# 带属性的标签
带属性的标签可以以简单的标签隐藏复杂的逻辑,如连接数据库等
1. 标签处理类中定义成员变量并提供相应的getter/setter,即相当于标签的属性
    ```java
    // 2. 成员变量代表标签属性,必须提供相应的getter/setter方法
    private String username;
    private String password;
    public void setUsername(String username){
        this.username=username;
    }
    public String getUsername(){
        return this.username;
    }
    public void setPassword(String password){
        this.password=password;
    }
    public String getPassword(){
        return this.password;
    }
    ```
2. 配置标签
    ```xml
    <!-- 带属性的标签 -->
    <tag>
        <!-- 定义标签名 -->
        <name>attrTag</name>
        <!-- 定义标签处理类 -->
        <tag-class>jwz.AttrTag</tag-class>
        <!-- 定义标签体为空 -->
        <body-content>empty</body-content>
        <!-- 配置username属性 -->
        <attribute>
            <!-- 属性名 -->
            <name>username</name>
            <!-- 是否为必需属性 -->
            <required>true</required>
            <!-- 是否支持JSP脚本、表达式等动态内容 -->
            <fragment>true</fragment>
        </attribute>
        <!-- 配置password属性 -->
        <attribute>
            <name>password</name>
            <required>true</required>
            <fragment>true</fragment>
        </attribute>
    </tag>
    ```
3. 使用自定义标签`<mytag:attrTag username="大傻逼" password="123456" />`

# 带标签体的标签
带标签体的标签可以在标签内嵌入其它内容[HTML+JSP],通常用于完成逻辑运算[判断/循环]
1. 标签处理类中的doTag方法控制标签体
    ```java
    String[] keys={"Java","C++","Ruby"};
    for (String key : keys ) {
        getJspContext().setAttribute("btName", key );
        // 输出标签体
        getJspBody().invoke(null);
    }
    ```
2. 配置标签的标签体不为空`<body-content>scriptless</body-content>`
3. 使用自定义标签`<mytag:bodyTag><button>${pageScope.btName}</button><br></mytag:bodyTag>`

# 以页面片段作为属性的标签
1. 定义类型为JspFragment的属性，代表页面片段
    ```java
    private JspFragment frag;
    public void setFrag(JspFragment frag){
        this.frag=frag;
    }
    public JspFragment getFrag(){
        return this.frag;
    }
    
    
    // 调用页面片段
    frag.invoke(null);
    ```
2. 配置标签体的页面片段属性,同普通属性一样
2. 使用标签库时，通过<jsp:attribtue />动作指令为标签的属性指定值
    ```jsp
    <mytag:fragmentTag>
        <jsp:attribute name="frag">
            <button>按钮</button>
        </jsp:attribute>
    </mytag:fragmentTag>
    ```

# 带动态属性的标签
动态属性标签:需要传入自定义标签的属性个数不确定，属性名也不确定
1. 定义标签处理类
    ```java
    package jwz;

    import javax.servlet.jsp.tagext.*;
    import javax.servlet.jsp.*;
    import java.io.*;
    import java.util.*;
    
    // 1. 继承javax.servlet.jsp.tagext.SimpleTagSupport
    // 2. 实现DynamicAttributes接口
    public class DynaAttrTag extends SimpleTagSupport implements DynamicAttributes{
        // 3. 保存每个属性名的集合
        private ArrayList<String> keys = new ArrayList<String>();
        // 3. 保存每个属性值的集合
        private ArrayList<Object> values = new ArrayList<Object>();
    
        // 4. 重写doTag()方法，负责生成页面内容
        public void doTag() throws JspException,IOException{
            for (int i=0;i<keys.size();i++ ) {
                // 获取页面输出流，并输出字符串
                getJspContext().getOut().write(keys.get(i)+"="+values.get(i));
            }
        }
    
        // 5. 实现接口的方法，用于获取属性
        public void setDynamicAttribute(String uri, String localName, Object value )throws JspException{
            keys.add(localName);
            values.add(value);
        }
    }
    ```
- 配置标签的动态属性`<dynamic-attributes>true</dynamic-attributes>`
- 使用标签时传入任意属性键值对即可
