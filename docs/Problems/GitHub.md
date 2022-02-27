---
title: GitHub 访问速度太慢的终极解决办法
permalink: how-to-access-github-quickly
description: 本文主要记录如何加快 GitHub 的访问速度
keywords: GitHub,clone,慢
quicklink: true
categories:
- Problems
tags:
- Problems
- Tools
- GitHub
- Git
---

1. 网上大部分资料都是通过 [IP Address](https://www.ipaddress.com/) 获得 IP,然而不仅没有效果，还会变得更慢

   正确姿势应该是通过命令行 ping 拿到 GitHub 相关域名的 IP, 修改 hosts 文件

   ```bash
    31.13.65.18 github.global.ssl.fastly.net
    13.250.177.223 github.com
   ```

2. 设置代理（可选）

    ```bash
    git config --global http.https://github.com.proxy 'socks5://127.0.0.1:1080'
    git config --global https.https://github.com.proxy 'socks5://127.0.0.1:1080'
    ```

3. 经过上面的步骤，速度应该会快一些，但是 SSH 方式克隆还是会比较慢

    可以通过 https 方式克隆，之后再修改 remote 地址

    ```bash
    git remote set-url origin git@github.com:username/repository.git
    ```