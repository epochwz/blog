# MySQL基础

**登录**

```bash
mysql [Options]
-u             用户名
-p             密码
-h             服务器
-P             端口号
-D             打开指定数据库

--delimiter    修改分隔符
--prompt       修改提示符
```

**退出**`exit`

**查看版本信息**`msyql -V`

**修改MySQL提示符**

```bash
# 连接客户端时指定
> mysql -u user -p pass --prompt 提示符
# 已登录时指定
> prompt 提示符

提示符
\D    完整日期
\d    当前数据库
\h    当前主机
\u    当前用户
```

**常用命令**

```bash
# 显示当前MySQL服务器版本
> SELECT VERSION();
# 显示当前日期时间
> SELECT NOW();
# 显示当前用户
> SELECT USER();
```

**MySQL规范**

* 关键字与函数名称全部大写
* 数据库名称、表名称、字段名称全部小写
* SQL语句必须以分号结尾