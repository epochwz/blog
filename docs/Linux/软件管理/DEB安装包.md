# DEB安装包

**apt-get在线安装**

* **apt-get：**使用官方服务器存放所有rpm软件包，自动解决依赖性问题
* **软件源配置文件：**`/etc/apt/sources.list`
* **软件源配置格式：**`deb [web或ftp地址] [发行版名字] [main/contrib/non-free]`

```
# apt-get安装命令格式
$ apt-get [Command] [Options] [Params]

# 安装
$ apt-get install 软件名称
# 删除
$ apt-get remove 软件名称
# 搜索
$ apt-cache search 软件名称
# 更新软件源
$ apt-get update
# 更新已安装的软件包
$ apt-get upgrade
# 系统更新
$ apt-get dist-upgrade

# 删除软件备份
$ apt-get clean
# 自动删除软件备份
$ apt-get autoclean

# 检查是否有损坏的依赖
$ apt-get check

[Options]
-c        指定配置文件
-purge    同时删除配置文件

[Params]
xxx.deb    指定要操作的deb包
管理指令    对deb包的管理操作
```

### dpkg本地安装

```
dpkg [Options] [Params]

[Options]
-i(install)   安装软件包
-r(remove)    删除软件包
-l(list)      显示已安装软件包列表

-P            删除软件包的同时删除其配置文件

-L            显示与软件包关联的文件
-c            显示软件包内文件列表

--configure   配置软件包
--unpack      解压软件包
[Params]      指定要操作的deb包


# 查找与"wps"相关联的软件包
$ dpkg -l | grep -i "wps"
```