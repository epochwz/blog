# 使用 vsftpd 搭建 FTP 文件服务器

---

> [vsftpd] 是一款完全免费、开源的 FTP 服务器软件，具备小巧轻快，安全易用的特点，还支持虚拟用户、宽带限制等功能。

本文就简单地介绍一下如何使用 `vsftpd` 在 `Ubuntu` 上快速搭建一个支持以下功能的 FTP 文件服务器

- 允许 **匿名用户** `ftp / anonymous` 查看 / 下载文件，文件目录是默认的 `/srv/ftp`
- 允许 **普通用户** 上传 / 下载 / 删除文件，文件目录是该用户主目录 `/home/username`
- 无论什么用户，都必须加入 **白名单文件** `/etc/user_list`, 才可以访问 FTP 服务器
- 禁止所有用户切换到上级目录 (只能操作属于自己的目录)

如果在安装和使用的过程中遇到任何错误，请参考 [Vsftpd 常见问题][vsftpd-problems]

## 安装配置

1. 安装

   ```bash
   sudo apt-get -y update && sudo apt-get install -y vsftpd
   ```

2. 验证

   ```bash
   vsftpd -v
   ```

3. 自定义配置：复制下面的内容替换掉 vsftpd 的默认配置文件 `/etc/vsftpd.conf`

    ```bash
    # ========================= 全局配置 =========================
    # 允许 / 禁止 登录用户具有写权限
    write_enable=YES
    # 允许 / 禁止 登录用户下载文件『如果禁止，那么所有的文件不能下载到本地，文件夹不受影响』
    # download_enable=YES
    
    # 使用系统本机时间
    use_localtime=YES
    # 使用 UTF-8 文件系统
    utf8_filesystem=YES
    
    # 允许 / 禁止 FTP 根目录具有 write 权限
    allow_writeable_chroot=YES
    
    # 指定在执行 ls -a 时显示文件的 UID、GID(NO) 或者 用户名、用户组 (YES)
    text_userdb_names=YES
    
    # ========================= 匿名用户配置 =========================
    # 允许 / 禁止 匿名用户登录
    # anonymous_enable=YES
    # 允许 / 禁止 匿名用户免密登录
    no_anon_password=YES
    # 指定匿名用户登录时使用的用户名
    # ftp_username=ftp
    # 指定匿名登录的 文件目录『不允许 具备 777 权限的目录』
    # anon_root=/srv/ftp
    
    # ========================= 本地用户配置 =========================
    # 允许 / 禁止 本地用户登录
    local_enable=YES
    # 指定本地用户登录的 文件目录
    # local_root=/home/username # 默认是该 Linux 用户的主目录
    
    # 本地用户 新增档案的 umask 值
    local_umask=022 # 对应文件权限是 700
    # 本地用户 上传档案的 文件权限
    file_open_mode=0755
    
    # ========================= 权限控制 =========================
    
    # 是否禁止用户切换到文件根目录的上级目录
    # YES: 禁止所有用户切换目录，仅在启用 chroot_list_enable 时允许 /etc/chroot_list 中的用户切换目录
    # NO: 允许所有用户切换目录，仅在启用 chroot_list_enable 时禁止 /etc/chroot_list 中的用户切换目录
    chroot_local_user=YES
    # 启用 / 关闭 切换目录的 黑 / 白名单
    # chroot_local_user=YES 时是白名单
    # chroot_local_user=NO 时是黑名单
    # chroot_list_enable=NO
    # 指定 切换目录的  黑 / 白名单 的文件路径
    # chroot_list_file=/etc/chroot_list
    
    # ========================= 访问控制 =========================
    
    # 全局黑名单文件 /etc/ftpusers（最高优先级配置文件）
    
    # 启用 / 关闭 用户登录的 黑 / 白名单文件
    userlist_enable=YES
    # 指定 用户登录的 黑 / 白名单 文件路径
    userlist_file=/etc/user_list
    # 指定 userlist_file 是黑名单还是白名单
    # YES: userlist_file 是黑名单，除该名单中的用户之外其他用户都能登录
    # NO:  userlist_file 是白名单，除该名单中的用户之外其他用户都不能登录
    userlist_deny=NO
    
    # ========================= 连接和端口配置 =========================
    
    # 启用 / 关闭 standalone 模式『如果关闭，则 vsftpd 不以独立模式运行，受到 xientd 服务管控，功能受限制』
    listen=YES
    # 指定 FTP 服务器建立连接所监听的端口
    # listen_port=21
    # 是否使用 20 端口进行数据传输
    # connect_from_port_20=YES
    
    # 是否启用 PASV 工作模式『如果不启动，则使用默认的 PORT 工作模式』
    # pasv_enable=YES
    # 监听特定 IP 的用户的 FTP 请求，如果不设置，则监听所有 IP 的请求『该配置只在 standlone 模式下生效』
    # pasv_address=
    # PASV 模式数据连接的 最大 / 最小 端口
    pasv_max_port=62000
    pasv_min_port=61000
    
    # ========================= 欢迎语配置 =========================
    
    # 启用 / 关闭 目录说明文件『使用者第一次进入某个目录时，检查该目录是否有 .message 文件，该文件放置了该目录的说明语』
    dirmessage_enable=YES
    # 指定目录说明文件
    message_file=.message
    
    # 指定用户登录欢迎语
    ftpd_banner=Welcom to username's FTP Server!
    ```

4. 重启

   ```bash
   sudo service vsftpd restart
   ```

5. 配置防火墙，开放端口 `20、21、61000-62000`
   - 如果是本地机器，需要在操作系统层面配置防火墙
   - 如果是云服务器，通常只需要在云服务器提供商提供的操作界面上配置安全组等类似防火墙的规则
6. 创建白名单文件 `/etc/user_list`: 只有该文件中的用户可以访问 FTP 服务器，一行写一个用户名

   ```bash
   touch /etc/user_list
   ```
7. 至此，我们的 FTP 文件服务器已经搭建完成。接下来只需要 [添加 FTP 用户](#添加用户)，并将其加入白名单就可以 [登录 FTP 服务器](#访问登录)了。

## 添加用户

### 匿名用户

vsftpd 安装后会默认创建 FTP 匿名用户 `ftp / anonymous`, 因此只需要将其加入白名单文件 `/etc/user_list` 即可

```bash
echo ftp >> /etc/user_list
echo anonymous >> /etc/user_list
```

### 普通用户

> FTP 非匿名用户其实就只是一个普通的 Linux 用户，只不过我们通常会出于安全考虑而禁止其 SSH 登录 Linux 系统的权限。

这里以添加 FTP 用户 `ftpuser` 作为示例

1. 添加一个 `禁止 SSH 登录 Linux 系统` 的 `shell`

   ```bash
   [ -z "$(grep ^/sbin/nologin$ /etc/shells)" ] && echo /sbin/nologin | sudo tee -a /etc/shells
   ```

2. 添加一个 `禁止 SSH 登录 Linux 系统` 的用户 `ftpuser`, 其主目录是 `/home/ftpuser`

   ```bash
   sudo useradd -m -d /home/ftpuser ftpuser -s /sbin/nologin
   ```

3. 设置其密码 `ftpuser_sapp`, 注意不要使用太简单的密码，否则系统不接受

   ```bash
   sudo echo ftpuser:ftpuser_sapp|chpasswd
   ```
4. 将用户 `ftpuser` 加入白名单文件

   ```bash
   echo ftpuser >> /etc/user_list
   ```

## 访问登录

> ! 强烈建议测试时使用专门的 FTP 客户端工具，不要使用命令行和浏览器，避免不必要的踩坑。如果还是踩了，可以参考一下 [客户端问题][client-problems]

> ! 客户端需要设置成使用 `被动模式 (PASSIVE MODE)`。这通常是 FTP 客户端的默认设置，但是如果登录失败了，应该首先考虑检查该项设置

- (不再推荐) ~~使用命令行~~ `ftp ip`
- (不再推荐) ~~使用浏览器~~ `ftp://ip`
- (强烈推荐) 使用 FTP 客户端工具，比如 `FileZilla`, `Intellij IDEA` 等

  **示例**：使用 `Intellij IDEA` 自带的 FTP 工具进行登录

  ![use-idea-login-ftp]

至此，一个既支持 `匿名用户 查看 / 下载 文件`、又支持 `普通用户 查看 / 上传 / 下载 文件` 的 FTP 服务器就搭建完成啦！

## 日常维护

- 配置文件 `/etc/vsftpd.conf`
- 日志文件 `/var/log/vsftpd.log`
- 常用命令

  ```bash
  # {启动 | 停止 | 重启 | 重载配置 | 查看运行状态}
  sudo service vsftpd {start|stop|restart|reload|status}
  ```

## 相关资源

> ! Ubuntu 上的 vsftpd 安装完成后，默认 **不支持匿名登录**，并且是以 **主动模式** 运行的，而大多数 FTP 客户端默认是使用 **被动模式** 进行登录的。了解 [FTP 的工作模式][ftp-working-mode]

因此，通常情况下我们都需要像本文一样使用自定义的配置，所以这里也贴心的准备了一份 [超级详细的 Vsftpd 配置介绍][vsftpd-configurations]

[vsftpd]:https://security.appspot.com/vsftpd.html
[client-problems]:/docs/tools/vsftpd/vsftpd-problems.md#客户端问题
[use-idea-login-ftp]: /docs/images/use-idea-login-ftp.png
[vsftpd-problems]:/docs/tools/vsftpd/vsftpd-problems.md
[vsftpd-configurations]:/docs/tools/vsftpd/vsftpd-configurations.md
[ftp-working-mode]: /docs/tools/vsftpd/ftp-working-mode.md
