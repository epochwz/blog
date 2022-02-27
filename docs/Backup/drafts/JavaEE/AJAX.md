# Ajax

Ajax(Asynchronous JavaScript And XML,异步的 JavaScript 和 XML)，可以在不刷新页面的前提下，进行页面局部更新，数据的动态加载

XMLHttpRequest 用于在后台与服务器交换数据，是 AJAX 的核心

Ajax 不是 W3C 的标准，不同浏览器的 XMLHttpRequest 有不同的创建方式

```js
// 1. 创建 XMLHttpRequest 对象
var ajax;
if(window.XMLHttpRequest){
    // IE7+,Firefox,Chrome,Opera,Safari
    ajax=new XMLHttpRequest();
}else{
    // IE6,IE5
    ajax=new ActiveXObject("Microsoft.XMLHTTP");
}

// 2. 发送请求

// 创建请求：请求方式,请求地址,异步(true)/同步(false)
ajax.open("GET","http://localhost:8080/ajax",true);
// 发送请求到服务器
ajax.send();

// 3. 处理响应
// 监听 AJAX 的执行过程
ajax.onreadystatechange=function(){
    // readyState 属性描述 XMLHttpRequest 当前状态
    // - 0 请求未初始化
    // - 1 服务器连接已建立
    // - 2 请求已被接收
    // - 3 请求正在处理
    // - 4 响应文本已被接收
    // ajax.status 属性描述服务器响应状态码
    if(ajax.readyState==4 && ajax.status==200){
        var response = ajax.responseText;
    }
}
```

同步

```js
// 1. 创建 XMLHttpRequest 对象
var ajax;
if(window.XMLHttpRequest){
    // IE7+,Firefox,Chrome,Opera,Safari
    ajax=new XMLHttpRequest();
}else{
    // IE6,IE5
    ajax=new ActiveXObject("Microsoft.XMLHTTP");
}

// 2. 发送请求

// 创建请求：请求方式,请求地址,异步(true)/同步(false)
ajax.open("GET","http://localhost:8080/ajax",false);
// 发送请求到服务器
ajax.send();
// 3. 处理响应
handleResponse();

function handleResponse (){
    // readyState 属性描述 XMLHttpRequest 当前状态
    // - 0 请求未初始化
    // - 1 服务器连接已建立
    // - 2 请求已被接收
    // - 3 请求正在处理
    // - 4 响应文本已被接收
    // ajax.status 属性描述服务器响应状态码
    if(ajax.readyState==4 && ajax.status==200){
        var response = ajax.responseText;
    }
}
```

jQuery AJAX 选项

| 常用设置项 | 作用                                                 |
| ---------- | ---------------------------------------------------- |
| url        | 请求地址                                             |
| type       | 请求类型 GET/POST                                    |
| data       | 请求参数                                             |
| dataType   | 服务器响应的数据类型 text/json/xml/html/jsonp/script |
| success    | 请求成功时的处理函数                                 |
| error      | 请求失败时的处理函数                                 |