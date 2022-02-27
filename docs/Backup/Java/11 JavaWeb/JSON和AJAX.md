# JSON

**JSON\(JavaScript Object Notation\)：**JavaScript对象表示法，是目前使用最广泛的、与开发语言无关的、轻量级的数据传输格式

**JSON优势：**易于人的阅读和理解，易于程序的解析和构建，相比XML更加轻量；易于前端解析：可以使用JavaScript内建方法直接解析成JavaScript对象

**JSON的数据类型**

* **数据结构：**对象\(Object\)、数组\(Array\)
* **基本数据类型：**字符串\(string\)、数值类型\(number\)、布尔类型\(true/false\)、空类型\(null\)
* JSON没有日期类型，通常使用字符串/时间戳等形式代替

**JSON的数据表示：**使用{}包含的键值对，key必须是string类型，value可以是任何JSON的数据类型

```
{
  // string: 字符串
  "name": "梦无涯",
  "age": 25,
  "weight": 25.6,
  "birthday": "1995-11-11",
  // boolean: 布尔类型
  "isRich": false,
  // null: 空类型
  "car": null,
  "house": null,
  "major": ["Java","Python"],
  "info": {
    "company": "神殇科技",
    "position": "创始人",
    "wage": 0.1
  },
  "comment": "JSON不支持任何形式的注释，常常使用一个新字段作为注释"
}
```

**JSON库：**映射 `Java Object` 和 `JSON数据格式`，常用的有`org.json`,`google-gson`

# AJAX

**AJAX\(Asynchronous JavaScript and XML\)：**异步的JavaScript和XML，通过HMLHttpRequest对象实现 在不重新加载页面的情况下进行客户端与服务端的数据交换，并使用JavaScript操作DOM，实现页面的局部更新

**同步：**客户端请求 -&gt; 服务端处理、响应 \| 客户端等待 -&gt; 客户端重载整个页面

**异步：**客户端请求 -&gt; 服务端处理、响应 -&gt; 客户端重载局部页面

# 跨域请求

**域名组成**

```
协议        子域名        主域名             端口          请求资源
http://     www    .    baidu.com    :    8080    /    index.html
```

**跨域请求：**当协议、子域名、主域名、端口号中任意一个不相同时，都算作不同域，不同域之间相互请求资源，称为**跨域请求**

**JavaScript同源策略：**JavaScript出于安全考虑，不允许跨域请求

**解决跨域方法**

* 代理
* JSONP