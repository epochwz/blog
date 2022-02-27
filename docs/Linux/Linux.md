# Linux

Linux 一切内容皆文件，不靠扩展名区分文件类型，区分大小写

## 系统脚本启动顺序

```bash
# 开机启动时立刻执行一次
0. /etc/rc*.d
   /etc/rc.local
# 任意用户每次登录时执行
1. /etc/environment
   PATH="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games"
2. /etc/profile
    . /etc/bash.bashrc
    if [ -d /etc/profile.d ]; then
      for i in /etc/profile.d/*.sh; do
        if [ -r $i ]; then
          . $i
        fi
      done
      unset i
    fi
3. ~/.profile
   ~/.bashrc
# 用户每次打开终端时执行
4. /etc/bash.bashrc
   /etc/bash_completion
   /usr/share/bash-completion/bash_completion
5. ~/.bashrc
   ~/.bash_aliase
# 用户每次退出登录时执行
6. ~/.bash_logout

login 时的执行顺序：/etc/environment -> /etc/profile -> /etc/bash.bashrc -> /etc/profile.d/ -> .profile -> ~/.bash*
bash  时的执行顺序：/etc/bash.bashrc -> ~/.bashrc -> ~/.bash_aliase -> ~/.bash_logout
```
## **系统运行级别**

```
# 查询系统运行级别
$ runlevel
# 修改系统运行级别
$ sudo vim /etc/inittab

# 系统运行级别
    0    关机
    1    单用户(安全模式，用于系统修复)
    2    不完全多用户，不含NFS服务(字符界面)
    3    完全多用户(字符界面)
    4    未分配
    5    图形界面
    6    重启
```

### 压缩 & 解压缩

### **.zip**

```
# 压缩文件
$ zip 压缩文件名 源文件
# 压缩目录
$ zip -r 压缩文件名 源目录
# 解压缩
$ unzip 压缩文件名

[Options]
-d    指定解压缩位置
```

### **.gz/.bz2**

```
# 压缩后源文件消失
$ gzip srcFile
$ bzip2 srcFile

# 压缩后保留源文件
$ gzip -c srcFile > desFile.gz
$ bzip2 -k srcFile

# 压缩目录下所有的子文件[不包括目录]
$ gzip -r dir
# .bz2无法压缩目录

# 解压缩
$ gzip -d srcFile.gz
$ bzip2 -d srcFile.bz2

# 解压缩后保留压缩文件
$ gunzip srcFile.gz
$ bunzip2 -k srcFile.bz2
```

### **.tar/.tar.gz/.tar.bz2**

```
$ tar [Options] [Files/dir]

[Options]
-c    打包
-x    解包
-t    查看压缩包

-v    显示过程
-f    指定操作的文件名
-C    指定解压缩位置

-z    .tar.gz
-j    .tar.bz2

# 打包并压缩成.tar.bz2
$ tar -jcvf /path/desFile.tar.bz2 srcFile

# 解压缩.tar.gz到指定目录
$ tar -zxvf srcFile.tar.gz -C /path/desDir
```
