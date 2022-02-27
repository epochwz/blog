---
title: Vsftpd 常见问题
permalink: problems-of-vsftpd
description: 本文主要记录 Vsftpd 的常见错误
keywords: vsftpd,ftp,linux,ubuntu
quicklink: true
categories:
- Problems
tags:
- Problems
- Tools
- FTP
- Vsftpd
- Ubuntu
- Linux
---

## FTP 根目录不允许有 write 权限

**问题描述**：登录时报错 `500 OOPS: vsftpd: refusing to run with writable root inside chroot()`

**问题原因**：vsftpd 服务器默认不允许 FTP 根目录具有 write 权限

**解决方法**（二选一)

- 禁止 FTP 根目录的写权限：`chmod -w $local_root` （会导致无法上传文件，不推荐）
- 修改 vsftpd 配置，使其允许 FTP 根目录具有写权限：`allow_writeable_chroot=YES`

## Windows 下使用命令行正常登录 FTP 服务器后，使用 `ls` 命令会卡死

**问题描述**：安装并配置好 vsftpd 服务器后（使用默认的 **PASV** 模式）, 使用浏览器或者其他 FTP 客户端都能够正常使用，而使用命令行则会卡死在 `ls` 命令上

```bash
root@vultr:~# ftp 33.120.55.182
ftp> ls
200 PORT command successful. Consider using PASV.
# 在这里就卡死了，也不会有报错信息
# --> Ctrl+C 强行停止后报以下信息
425 Failed to establish connection.
```

在网上找到的一些资料说是客户端要使用 PASV 被动模式登陆：`ftp -p $IP`

```bash
root@vultr:~# ftp -p 33.120.55.182
ftp> ls
# 注意：这里的确已经进入被动模式，且服务器 IP 是 145.34.125.208 , 这是我的内网 IP
227 Entering Passive Mode (145,34,125,208,239,253).
425 Failed to establish connection. # 一段时间后自动报该错误信息，仍旧不能获取到目录列表
```

可以看到，使用了被动模式来登录后依旧不能奏效。

后来在[这篇博客](https://blog.csdn.net/qq_38782611/article/details/83549766) 中找到了解决方法：在 vsfptd 配置文件 `/etc/vsftpd.conf` 中添加 `pasv_address=33.120.55.182（公网 IP）`

```bash
root@vultr:~# ftp -p 33.120.55.182
ftp> ls
227 Entering Passive Mode (33,120,55,182,241,255). # 注意这里的 IP 变成了 33.120.55.182, 这是我的公网 IP
150 Here comes the directory listing. # 成功获取目录列表
```

总结一下，解决这个问题我们总共干了下面两件事情

1. 配置 vsftpd 服务器 PASV 模式下监听的 IP 地址：`pasv_address=33.120.55.182`
2. 在客户端命令行登录时使用 PASV 模式：`ftp -p 33.120.55.182`

由此，可以看出这个问题是因为

1. 如果 vsftpd 服务器使用了 PASV 模式且不配置其 `pasv_address` 的话，可能会给 FTP 客户端返回内网 IP
2. **某些命令行**默认不使用被动模式登录 FTP，且无法自动获取 FTP 服务器的公网 IP
    - 这些不机智的命令行可能包括 **Windows** 和某些 **Linux Server** 自带的命令行
    - **FTP 工具** 往往会使用 PASV 模式，且在其返回的 IP 不可用的情况下自动替换成服务器公网 IP
