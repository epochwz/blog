# HTTP

## HTTP 请求方式

GET 是将数据追加在请求链接上显式地向服务器发送数据，常用于不包含敏感信息的查询操作

POST 是将数据存放在“请求体”中隐式地向服务器发送数据，常用于服务器写操作和其他安全性要求较高的功能

## HTTP 请求报文

HTTP 请求报文结构：请求行、请求头、请求体

- 示例：POST 请求报文

    ```bash
    # 请求行：请求方式、URL、协议版本
    POST /Learning/student HTTP/1.1

    # 请求头：Cookie、User-Agent, etc.
    Host: localhost:8080
    Connection: keep-alive
    Content-Length: 81
    Cache-Control: max-age=0
    Origin: http://localhost:8080
    Upgrade-Insecure-Requests: 1
    DNT: 1
    Content-Type: application/x-www-form-urlencoded
    User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/77.0.3865.90 Safari/537.36
    Sec-Fetch-Mode: navigate
    Sec-Fetch-User: ?1
    Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3
    Sec-Fetch-Site: same-origin
    Referer: http://localhost:8080/Learning/student.html
    Accept-Encoding: gzip, deflate, br
    Accept-Language: zh-CN,zh;q=0.9,en;q=0.8
    Cookie: JSESSIONID=8B00F238B47CA20918751E1045E85295; Idea-3f073365=d18d9372-e3c0-4383-be92-cd3580696243

    # 请求体 (Form Data): 请求数据
    name=%E7%8E%8B%E4%BA%8C%E9%BA%BB%E5%AD%90&age=12&sex=male&hobby=music&hobby=sport
    ```

- 示例：GET 请求报文

    ```bash
    # 请求头：请求方式、URL?Query String Parameters、协议版本
    GET /Learning/student?name=%E8%8B%B9%E6%9E%9C999&age=12&sex=female&hobby=music&hobby=learn HTTP/1.1

    # 请求头
    Host: localhost:8080
    Connection: keep-alive
    Upgrade-Insecure-Requests: 1
    DNT: 1
    User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/77.0.3865.90 Safari/537.36
    Sec-Fetch-Mode: navigate
    Sec-Fetch-User: ?1
    Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3
    Sec-Fetch-Site: same-origin
    Referer: http://localhost:8080/Learning/student.html
    Accept-Encoding: gzip, deflate, br
    Accept-Language: zh-CN,zh;q=0.9,en;q=0.8
    Cookie: JSESSIONID=8B00F238B47CA20918751E1045E85295; Idea-3f073365=d18d9372-e3c0-4383-be92-cd3580696243

    # 请求体（Query String Parameters）:可以有，但通常不使用
    # name=%E8%8B%B9%E6%9E%9C999&age=12&sex=female&hobby=music&hobby=learn
    ```

## HTTP 响应报文

HTTP 响应报文结构：响应行、响应头、响应体

```bash
# 响应行：协议版本、响应状态码
HTTP/1.1 200

# 响应头
Content-Length: 108                     # 请求体内容长度
Content-Type: text/html;charset=UTF-8   # 请求体内容类型：告知浏览器采用何种方式对响应体进行处理
Date: Sun, 13 Oct 2019 03:40:23 GMT

# 响应体：纯文本、图片、HTML、XML、JSON
```

## HTTP 常见状态码

| 响应状态码 | 状态码描述                               |
| ---------- | ---------------------------------------- |
| 200        | 服务器处理请求成功                       |
| 404        | 资源不存在                               |
| 500        | 服务器内部错误                           |
| 403        | 服务器拒绝访问                           |
| 301、302   | 请求重定向                               |
| 400        | 无效的请求                               |
| 401        | 未经过授权                               |
| 503        | 服务器超负载或正在停机维护，无法处理请求 |

## HTTP 常见 Content-Type

| MIME 类型                        | 描述           |
| -------------------------------- | -------------- |
| text/plain                       | 纯文本         |
| text/html                        | HTML 文档      |
| text/xml                         | XML 文档       |
| application/x-msdownload         | 需要下载的资源 |
| image/jpeg、image/gif、image/... | 图片资源       |
