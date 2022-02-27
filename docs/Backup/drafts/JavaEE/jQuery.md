# jQuery

JavaScript 库是第三方厂商为了简化 JavaScript 开发所封装的函数库

- jQuery
- Vue.js
- React
- AngularJS

jQuery 是一个轻量级 JS 库，使用十分简单，其核心是**选择器**，用于获取页面元素

- 提供了大量高效的方法，大幅提升开发速度
- jQuery 采用独立 JS 文件发布

## 选择器

Web 页面开发的两个要素

1. 要选择页面上的哪些元素
2. 对选择的元素做什么操作

jQuery 选择器用于选择需要操作的页面元素

- jQuery(selector-experssion)
- $(selector-experssion)

**基本选择器**

| 语法                     | 作用       |
| ------------------------ | ---------- |
| $("#id")                 | ID 选择器  |
| $("tag")                 | 标签选择器 |
| $(".class")              | 类选择器   |
| $("selector1,selector2") | 组合选择器 |

**层叠选择器**：根据元素的位置关系来选择元素的选择器表达式

| 语法           | 作用       |
| -------------- | ---------- |
| $("TagA TagB") | 后代选择器 |
| $("TagA>TagB") | 子选择器   |
| $("TagA~TagB") | 兄弟选择器 |

**属性选择器**：根据元素的属性值来选择元素的选择器表达式

| 语法                            | 作用                      |
| ------------------------------- | ------------------------- |
| $("selector[attribute=value]")  | 属性值等于 value 的元素   |
| $("selector[attribute^=value]") | 属性值以 value 开头的元素 |
| $("selector[attribute$=value]") | 属性值以 value 结尾的元素 |
| $("selector[attribute*=value]") | 属性值保护你 value 的元素 |

**位置选择器**：根据元素的位置来选择元素的选择器表达式

| 语法                | 作用               |
| ------------------- | ------------------ |
| $("selector:first") | 选择第一个元素     |
| $("selector:last")  | 选择最后一个元素   |
| $("selector:even")  | 选择偶数位置的元素 |
| $("selector:odd")   | 选择奇数位置的元素 |
| $("selector:eq(n)") | 选择指定位置的元素 |

**表单选择器**：根据表单元素类型来选择元素的选择器表达式

| 语法                   | 作用             |
| ---------------------- | ---------------- |
| $("selector:input")    | 选择所有输入元素 |
| $("selector:text")     | 选择文本输入框   |
| $("selector:password") | 选择密码输入框   |
| $("selector:submit")   | 选择提交按钮     |
| $("selector:reset")    | 选择重置按钮     |

## 操作元素属性

- attr(name [,attrValue]) 获取/设置元素属性,当选中多个元素时，返回第一个符合条件的元素
- removeAttr(name) 移除元素属性

## 操作元素的 CSS 样式

- css() 获取/设置匹配元素的样式属性
- addClass() 为每个匹配的元素添加指定的类名
- removeClass() 为每个匹配的元素删除全部或指定的类名

## 操作元素的内容

- val() 获取/设置输入项的值
- text() 获取/设置元素的纯文本
- html() 获取/设置元素内部的HTML

html与text的区别在于是否对html进行转义

## 处理事件

- on('event',function) 为选中的页面元素绑定事件
- click(function) 绑定点击事件
- 处理方法中提供了event 参数，包含事件的相关信息

常用事件

| 鼠标事件   | 键盘事件 | 表单事件 | 文档/窗口事件 |
| ---------- | -------- | -------- | ------------- |
| click      | keypress | blur     | load          |
| dbclick    | keyup    | change   | resize        |
| mouseenter | keydown  | focus    | scroll        |
| mouseleave |          | submit   | unload        |
| mouseover  |          |          |               |

$(this) 指当前事件产生的对象