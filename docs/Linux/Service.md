# Ubuntu 添加 开机自启动的 Service

## 什么是 Ubuntu 的 Service

网上的大部分资料说，Service 是 Linux 中开机自启动的后台程序。实际上，这个说法不是很准确，只不过是 Service 一般就是用于启动后台程序，且常常有开机自启动的需求，即所谓的"服务"。

而事实上呢，Service 其实就是一个 Linux Shell 命令，用来启动 `/etc/init.d/` 下的脚本；就像是`echo`命令用来输出字符串一样。

比如说， `service hello start` 相当于 `/etc/init.d/hello start`, 其中，hello 是脚本文件名，start 是传入脚本的参数

本文以 制作一个开启自启动的 `IDEA License Server`为例，简单介绍 Service 用法

## 如何添加一个 Service

1. 在 `/etc/init.d` 目录下新建一个脚本文件,内容如下

    `sudo touch /etc/init.d/hello`

    ```bash
    #!/bin/bash
    echo hello
    ```

2. 赋予它执行权限 `sudo chmod +x /etc/init.d/hello`
3. 此时系统就拥有一个

## 控制服务开机自启动

添加软链接 `/etc/rc{RUNLEVEL}.d/S{PRIORITY}{SERVICENAME}`, 指向 /etc/init.d/

或者

sudo update-rc.d idea-server defaults
sudo update-rc.d idea-server remove
