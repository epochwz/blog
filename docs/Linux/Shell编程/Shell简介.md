# Shell简介

Shell是**命令行解释器：**用户和Linux内核沟通的桥梁

* Shell&lt;-&gt;用户命令&lt;==&gt;ASCII码表&lt;==&gt;机器语言&lt;-&gt;内核

Shell是**脚本语言：**易编写、易调试、灵活性强，能够直接调用Linux系统命令

**Shell的语法类型**

* Bourne Shell：起源于Unix，其主文件名为sh\(sh,ksh,**Bash**,psh,zsh\)
* C Shell：起源于BSD版的Unix系统，语法类似C语言\(csh,tcsh\)
* Linux标准Shell:Bash
* Unix标准Shell:csh

**查看和调用当前系统的Shell类型**

```
# 查看Linux当前运行的Shell类型
$ echo $SHELL
# 查看Linux支持的Shell语法类型
$ vim /etc/shells

# Linux支持多级调用Shell
# eg. bash中调用bash
# eg. bash中调用其他类型Shell
$ shellName
```

**执行脚本**

```
# 编写脚本
$ vim HelloWorld.sh
    #!/bin/bash    # 该行表示后续内容为Linux标准脚本
    # The First Program
    echo "Hello World!"

# 赋予执行权限
$ chmod 500 HelloWorld.sh

# 执行脚本
$ ./HelloWorld.sh
# 执行脚本(无须执行权限)
$ bash HelloWorld.sh
```

**Shell特殊字符**

    #     注释
    ''    单引号中的特殊字符均失效
    ""    双引号中的特殊字符除了 [$引用变量的值]、[引用命令`]、[转义字符\] 以外均失效
    \     转义字符,使特殊字符失效

    ``    引用系统命令，并保存命令执行的输出，Bash会优先执行
    $()   引用系统命令，并保存命令执行的输出，Bash会优先执行
    $     调用变量的值



