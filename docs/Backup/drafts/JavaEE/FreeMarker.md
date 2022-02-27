# FreeMarker

模板引擎：数据+模板=结果，实现数据与展示“解耦”

主流模板引擎：JSP，FreeMarker，Beetl

FreeMarker Language Template,FreeMarker 脚本语言

|          | JSP     | FreeMarker |
| -------- | ------- | ---------- |
| 官方标准 | 是      | 否         |
| 执行方式 | 编译型  | 解释型     |
| 执行效率 | 高      | 低         |
| 开发效率 | 低      | 高         |
| 扩展能力 | 弱      | 强         |
| 数据提取 | JSTL+EL | 内置标签   |

**FTL 取值**

```java
// 基本数据类型
${attrName}                 输出属性值
${attrName!defaultValue}    指定默认值
${attrName?string}          格式化输出
// 引用数据类型
${obj.attr}
${obj.map["key"]}
```

**条件判断**

```java
<#if expression>
statement;
</#if>

<#switch var>
    <#case value>
        <#break>
    <#default>
        <#break>
<#/switch>
```

**集合迭代**

```java
<#list objs as obj>
    ${obj_index}-${ojb.attr}
<#list>

<#list map?keys as key>
    ${key}-${map[key]}
<#list>
```

**内置函数**

![内置函数](../../resources/images/javaweb/freemarker-funs.png)

[Manual](http://www.kerneler.com/freemarker2.3.23/index.html)

**三目运算符**

`${boolean?string(attr1,attr2)}`