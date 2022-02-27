# Web

## 软件结构发展史

|          | 单机时代                           | 联机时代                       | 互联网时代                             |
| -------- | ---------------------------------- | ------------------------------ | -------------------------------------- |
| 应用架构 | 桌面应用                           | C/S(Client-Server)             | B/S(Browser-Server)                    |
| 优点     | 容易使用，结构简单                 | 数据共享方便，安全性高         | 数据共享方便，无需安装客户端，开发简单 |
| 缺点     | 数据难以共享，更新不及时，安全性差 | 必须安装客户端，升级与维护困难 | 执行速度和用户体验相对较弱             |

## Cookie

Cookie 是浏览器保存在本地的文本内容，会伴随请求一起发送给 Web 服务器，通常用于保存登录状态、用户信息等短小的文本信息

Cookie 具有时效性，如果没有设置有效时间，则默认是在当前会话中有效，当关闭浏览器（所有窗口）时失效

## Session

Session 用于保存与“浏览器窗口”对应的数据，也称“用户会话”

Session 的数据存储在 Web 服务器的内存中，具有时效性

- eg. Tomcat 中如果 30 分钟内没有再次发送请求则该 Session 被销毁

Session 的实现原理

1. 当浏览器首次向 Web 应用发送请求时，Web 服务器会自动创建 Session 对象并将其 JSESSIONID 写入到 Cookie 中
2. 当浏览器再次向 Web 应用发送请求时，会携带 Cookie 中的 JSESSIONID 一起发送给 Web 服务器
3. 此时 Web 服务器可以通过 JSESSIONID 获取 Session, 并从中提取用户数据
4. 当关闭浏览器并重新打开时，将开启一次新的会话，相当于第一次访问 Web 应用

## Cookie & Session

Cookie 存储在客户端（浏览器），Session 存储在服务器（Tomcat）

Cookie 是浏览器最底层的实现，而 Session 则是基于 Cookie 实现的

MVC(Model-View-Controller,模型-视图-控制器)

JavaWeb 开发历程

1. Servlet
   - 配置麻烦
   - 页面输出麻烦
2. JSP
   - 处理、封装数据麻烦
3. JSP+JavaBean: 模式一，适合开发小型案例
   - 优点：使用 JavaBean 封装和处理数据，使用 JSP 显示数据
   - 缺点：JSP 调用 JavaBean 很随意，后期维护麻烦
4. Servlet+JSP+JavaBean: 模式二，MVC设计模式
   - 将用户的请求数据交给Servlet处理，Servlet 调用JavaBean封装和处理数据，使用JSP展示结果

## 文件上传

文件上传：将本地磁盘文件通过 IO写入到服务器的过程

文件上传三要素

- 表单提交方式必须是POST
- 表单中必须有文件上传表单项，必须有name属性
  - `<input type="file" name="filename">`
- 表单的 `enctype` 必须是 `multipart/form-data`

文件上传的技术

- Servlet 3.0
- JSPSmartUpload
- FileUpload
- 框架
