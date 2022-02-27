# MySQL

**启动**`sudo service mysql start`

**停止**`sudo service mysql stop`

**安装位置**`/etc/mysql/my.cnf`

**配置字符集**

```bash
# CentOS
# /etc/my.cnf
# Ubuntu
# /etc/mysql/my.conf.d/mysqld.cnf
   [mysql]
   default-character-set=utf8
   [mysqld]
   character-set-server=utf8

# 注：中文乱码问题
v5.1
   [mysql] default-character-set=utf8
   [mysqld]default-character-set=utf8
v5.5
   [mysql] default-character-set=utf8
   [mysqld]character-set-server=utf8
```

**配置MySQL**

```bash
# 登录mysql
> mysql -u root
# 修改root密码
> set password for root@localhost=password('yourPass');

# 查看mysql用户列表
> select user,host from mysql.user;
# 删除匿名用户
> delete from mysql.user where user='';

# 插入新用户
> insert into mysql.user(Host,User,Password) values("localhost","yourName",password("yourPass"));
> flush privileges;

# 创建新的DataBase
> CREATE DATABASE `mMall` DEFAULT CHARACTER SET utf8 COLLATE utf8_general_ci;

# 赋予本地用户所有权限
   # 操作权限
   > grant all privileges on mmall.* to yourName@localhost identified by 'yourPass';
   # 外网权限
   > grant all privileges on mMall.* to 'yourName'@'%' identified by 'yourPass'
   > flush privileges

# 导入SQL文件
> use dbName
> source xxx.sql
```