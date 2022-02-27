# SSH 协议入门

## SSH 简介

> **SSH**(Secure SHell) 是一种使用了**非对称加密算法**的网络传输加密协议，主要用于在网络服务中实现 **安全传输** 和 **身份验证**。

举个栗子：

- 远程登录 Linux 系统时使用的 `ssh username@host` 就是基于 SSH 协议的身份验证方式
- GitHub 克隆项目时也可以选择使用 SSH 协议的加密传输方式 `git clone git@github.com:username/repo.git`

无论是 Linux 服务器，还是 GitHub 的使用，都是程序猿的必备技能。因此，本文基于 **Ubuntu 16.04** 和 **GitHub** 简单介绍 如何配置密钥对 来进行登陆验证。

## SSH 认证初体验

当你要登录到一台支持 SSH 登录的服务器时，只需要在命令行中执行`ssh username@host`

```bash
# username  代表 Linux 系统用户的用户名
# host      代表服务器主机地址，可以是 IP，也可以是域名
# 示例
ssh root@39.104.55.142
ssh git@github.com
```

如果是第一次登录该服务器，系统会给出以下提示：

```bash
The authenticity of host 'host (39.104.55.142)' can’t be established.
RSA key fingerprint is xxxxxx
Are you sure you want to continue connecting (yes/no)?
# 无法确认该主机的真实性，该主机的 RSA 公钥指纹是 xxxxxx，你是否确定继续连接该服务器？
```

你可以通过提示信息中的`RSA key fingerprint`去验证服务器的真实性。通常我们登录的服务器都是知根知底的，因此输入`yes`继续登录即可。

此时系统会提示你输入密码，至此，你就完成了一次基于**密码认证**方式的 SSH 登录

## SSH 密钥对认证

通过密码认证的方式来远程登录，每次都得输入密码，显得十分繁琐。因此 SSH 协议提供了**密钥对认证**方式。

一个密钥对分为**公钥**和**私钥**两部分，持有私钥的客户端机器能够登录持有对应公钥的服务器，而无需输入密码。

- 公钥文件默认存储在`$HOME/.ssh/authorized_keys`文件中。
- 私钥文件默认存储在`$HOME/.ssh/id_rsa`文件中。

以下简单介绍如何生成并使用密钥对来登录服务器

**一、生成密钥对**

1. 创建当前用户的 `.ssh` 目录：`mkdir ~/.ssh`
2. 进入当前用户的 `.ssh` 目录：`cd ~/.ssh`
3. 生成密钥对：`ssh-keygen -t rsa`
4. 根据提示输入相关信息（通常全部回车默认即可)
5. 默认最终生成以下文件
   - 私钥文件 `id_rsa`
   - 公钥文件 `id_rsa.pub`

**二、将公钥存放到服务器**

将公钥文件 `id_rsa.pub` 文件重命名为 `authorized_keys` 并置于服务器的 `$HOME/ssh` 目录下。

**Tips:** 你也可以使用以下命令直接将本地公钥文件 `id_rsa.pub` 的内容复制到服务器上

```bash
ssh username@host 'mkdir -p .ssh && cat >> .ssh/authorized_keys' < ~/.ssh/id_rsa.pub
```

**Tips:** `authorized_keys` 文件支持存储多个公钥，只需要将其他`id_rsa.pub`的内容复制到该文件即可，**一行一个**

**三、将私钥存放到客户端**

将`id_rsa`文件存放到客户端机器的`$HOME/.ssh`目录下

**四、设置文件权限**

密钥文件和`.ssh`目录都需要设置相应的文件权限，否则文件权限过高将会导致验证失败。

```bash
chmod 755 /home/%username%
chmod 775 /home/%username%/.ssh
chmod 664 /home/%username%/.ssh/authorized_keys
chmod 600 /home/%username%/.ssh/id_rsa
```

至此，便可在命令行中执行 `ssh user@host` 直接登录服务器了

## 配置 GitHub SSH 密钥对

事实上 GitHub 可以看作一个服务器，因此我们也可以使用 `ssh git@github.com` 来登录 GitHub

```bash
PTY allocation request failed on channel 0
Hi s3kj-josena! You’ve successfully authenticated, but GitHub does not provide shell access.
Connection to github.com closed.
```

由提示信息可见，我们使用 ssh 登录了 GitHub 服务器并被验证成功，但出于安全问题，GitHub 服务器并不提供可登录的 shell，因此关闭了我们的 connection

明白了 GitHub 与服务器的相似之处，那么为 GitHub 配置密钥对认证就十分简单了

1. 生成密钥对（同上述）
2. 将公钥文件配置到 GitHub 上

   - 登录GitHub后通过 `Settings -> SSH and GPG keys -> New SSH` 依次进入 `SSH Key` 的添加页面

    ![Add SSH Key][]
3. 将私钥文件配置到自己的电脑上(同上述)
4. 使用 SSH 方式克隆项目：`git clone git@github.com:username/reponame.git`

## SSH 连接超时设置

`SSH Service` 默认有超时断开机制:若 SSH 登录后长时间不进行操作，客户端连接便会被自动断开，可以通过修改 服务器 **或** 客户端 的 SSH 配置，避免 SSH 连接超时断开。

**服务器设置**

1. 修改 `SSH Service` 的配置文件 `/etc/ssh/sshd_config`, 添加以下内容

    ```bash
    # 定义超时的间隔时间 (s)
    ClientAliveInterval 120
    # 定义允许的最大超时次数
    ClientAliveCountMax 30
    # --> 允许的总超时时间：120s * 30 次 = 3600s = 1h
    ```

2. 重启 `SSH Service` 服务：`sudo service sshd restart`

**客户端设置**

1. 修改 SSH 配置文件 `/etc/ssh/ssh_config`, 添加以下内容

    ```bash
    # 每 60s 向服务器发送 KeepAlive 请求，避免被强制断开
    ServerAliveInterval 60
    ```

2. 重载配置文件，使之生效：`source /etc/ssh/ssh_config`

## 在客户端配置多个私钥

SSH 默认使用 `~/.ssh/id_rsa` 作为私钥文件，如果要**使用不同的私钥文件访问不同的服务器**，可以通过配置文件 `~/.ssh/config` 来实现。

常用的配置选项如下：

```bash
Host    　                  #　主机别名
HostName　                  #　服务器真实地址
IdentityFile　              #　私钥文件路径
PreferredAuthentications　  #　认证方式
User　                      #　SSH 用户名（可选）
```

**Tips:** 未在配置文件中进行配置的主机地址，依旧使用默认的 `~/.ssh/id_rsa` 私钥文件进行验证

示例：

```bash
# 配置访问服务器 39.104.55.142 时使用私钥文件 id_rsa_epoch --> ssh root@ecs == ssh root@45.77.163.73
Host ecs
HostName 39.104.55.142
IdentityFile ~/.ssh/id_rsa_epoch
PreferredAuthentications publickey
User root
# 配置访问 GitHub 时使用私钥文件 id_rsa_josena --> ssh username@github.com == ssh username@github.com
Host github.com
HostName github.com
IdentityFile ~/.ssh/id_rsa_josena
PreferredAuthentications publickey
```

[Add SSH Key]:../images/Add-SSH-Key.jpg