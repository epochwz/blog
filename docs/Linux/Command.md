# Command

## 常用命令

**命令行快捷键**

```
Ctrl+a    行首
Ctrl+e    行尾
Ctrl+u    清除命令
Ctrl+z    后台执行命令
Ctrl+r    搜索历史命令
```

**关机&重启&休眠&挂起**

```
# 安全关机
$ sudo shutdown -h(halt) [时间]
# 安全重启
$ sudo shutdown -r(reboot) [时间]
# 取消关机/重启
$ sudo shutdown -c(cancle)
# 休眠
$ sudo pm-hibernate
# 挂起
$ sudo pm-suspend
```

**文件下载**

```
$ wget -c -t 0 -O newFileName URL
[Options]
-c    断点续传
-t n  重试次数，0则为不限次数
-O    保存文件的名字
URL   下载网址
```

## 帮助命令

**官方帮助文档**

```
# 查看官方帮助文档(支持 类似vim中/ 的查找操作)
$ man Command

# 查看命令拥有的帮助级别
$ man -f Command
$ whatis Command

# 查看命令相关的所有帮助文档
$ apropos Command
$ man -k Command
```

**其他帮助命令**

```
# 查看命令的帮助文档
$ Command --help

# Shell是Linux的命令解释器
# 如何判断是否是Shell内部命令：执行 whereis <Command> 能够显示命令所在路径的则不是 Shell 内部命令
# 查看Shell内部命令的帮助文档
$ help shellCommand
```

## 历史命令

**查看历史命令**

```
# 查看历史命令
$ history
$ cat ~/.bash_history

# 历史命令配置文件~/.bash_history
$ history [Options] ~/.bash_history
[Options]
-c    清空历史命令
-w    将缓存中的历史命令写入历史命令保存文件~/.bash_history
```

**调用历史命令**

```
上下箭头调用历史命令
!n    执行第n条历史命令
!!    执行上一条命令
!str  执行最后一条以str开头的命令
```
