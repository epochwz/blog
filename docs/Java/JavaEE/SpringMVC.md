# SpringMVC

## 项目实战

1. SpringMVC 全局异常：防止项目代码信息、SQL 语句的泄露
2. Spring & SpringMVC 包扫描隔离
3. @Component
4. ModelAndView 不在作用于服务器转发（页面跳转），而是返回包装后的 json 对象

## Lombok

优点：

- 提高编码效率
- 消除冗长代码
- 避免修改字段名称却忘了修改方法名

注意点：必须在 IDE 层面进行 Lombok 的支持，否则 IDE 会报错

原理

- `javac` 从 JDK6 开始支持 `JSR 269 Pluggable Annotation Processing API`, 只要程序实现了该 API, 就能在 `javac` 运行的时候得到调用
- 源代码 -> 编译 -> AST -> Annotation Processing(Lombok Annotation Processor) ->

`mvn clean package -Dmaven.test.skip=true -Pprod`

## SpringMVC 拦截器

### 配置
```xml
<mvc:interceptors />
<mvc:interceptor />
<mvc:exclude-mapping path="" />
<mvc:mapping path="" />
/** 所有路径及其子路径
/*  当前路径下的所有路径，不包含子路径
/   web 项目的根目录请求
```
### 重置 HttpServletResponse

### 解决拦截登录循环问题

![SpringMVC Interceptor](/docs/images/java/springmvc-interceptor-process.jpg.png)
![Lombok](/docs/images/java/lombok-princple.jpg.png "Lombok 实现原理")
