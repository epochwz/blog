# EL & JSTL

## EL

EL(Expression Language,表达式语言)，用于简化 JSP 的输出

基本语法：`${expression}` -> `${[作用域.]属性名[.子属性]}`

四个作用域对象

| 对象               | 功能            |
| ---------------- | ------------- |
| pageScope        | 从当前页面取值       |
| requestScope     | 从当前请求中获取属性值   |
| sessionScope     | 从当前会话中获取属性值   |
| applicationScope | 从当前应用中获取全局属性值 |
| param            | 从请求参数中取值        |

不指定作用域对象时，将自动按作用域从小到大地尝试取值

EL 支持将运算结果进行输出

EL 支持直接输出对象，调用 toString

输出参数值：${param.paramName}

## JSTL

JSTL(JSP Standard Tag Library,JSP 标准标签库)，用于简化 JSP 开发，提高代码的可读性和可维护性

JSTL 由 SUN(Orcale)定义规范，由 Apache Tomcat 团队实现

JSTL 1.2.5 组件

| 组件名称                              | 描述              |
| --------------------------------- | ------------- |
| taglibs-standard-spec-1.2.5.jar   | 标签库定义包（必选）    |
| taglibs-standard-impl-1.2.5.jar   | 标签库实现包（必选）    |
| taglibs-standard-jstlel-1.2.5.jar | EL表达式支持包 （备选） |
| taglibs-standard-compat-1.2.5.jar | 1.0版本兼容包（备选）  |

JSTL 标签库种类

- 核心标签库 core
  - JSTL 最重要的标签库，提供了 JSTL 的基础功能
  - `<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>`
  - JSTL 核心标签库在 `taglibs-standard-impl-1.2.5.jar` 里的 `META-INF/c.tld` 中定义
  - 便捷输出标签：`<c:out value="" default="" escapeXml="true">`
- 格式化输出标签库 fmt
  - `<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/fmt" %>`
  - `<fmt:formatDate pattern="" value="">`
  - `<fmt:formatNumber pattern="" value="">`
- SQL 操作标签库 sql
- XML 操作标签库 xml
- 函数标签库 functions