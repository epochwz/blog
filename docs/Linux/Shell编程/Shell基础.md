# Shell基础

**Linux命令的基本格式**

```
root@localhsot:~# command [Options] [Params]

[Command prompt]
root         # 用户名
localhost    # 主机名
~            # 当前所在目录
#/$          # 超级用户/普通用户
```

**echo输出命令**

```
# 输出内容到标准输出
$ echo [Options] [输出内容]

# 支持颜色输出
$ echo -e "\e[1;颜色代码 输出内容 \e[0m"

-e    支持转义字符
```

**接收键盘输入**

```
$ read [Options] [varName]
[Options]
-p "message"    输出提示信息
-s              隐藏用户输入的数据
-t num          指定等待输入时间
-n num          允许用户输入的字符数,超过后直接执行命令

[varName]       接收用户输入的参数
```

**管道符**

```
# Command1的正确输出作为Command2的操作对象
$ Command1 | Command2
```

**命令别名**

```
# 查看系统当前生效的别名
$ alias
# 修改别名
$ alias Command='Command [Options]'
# 删除别名
$ unalias Command

# 永久修改别名(修改环境变量配置文件)
$ vim ~/.bashrc
```

**命令搜索命令whereis和which**

```
# 查看命令及帮助文档所在路径
$ whereis 命令名
-b    # 只查找可执行文件
-m    # 只查找帮助文件

# 查看命令所在路径及其别名
$ which fileName

# whereis和which只能查找系统命令，无法查找shell命令、外来命令
```

**命令优先级**

```
1    绝对路径/相对路径执行的命令
2    别名
3    Bash内部命令
4    $PATH定义的目录查找顺序
```

**多命令顺序执行**

| **Command1**  ;    **Command2** | 命令之间没有逻辑关系，全部顺序执行 |
| :--- | :--- |
| Command1 && Command2 | Command1正确执行时才能执行Command2 |
| Command1 \|\|    Command2 | Command1执行错误时才能执行Command2 |
