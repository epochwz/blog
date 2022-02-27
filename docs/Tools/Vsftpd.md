# Vsftpd

> [vsftpd] 是一款完全免费、开源的 FTP 服务器软件，小巧轻快，安全易用，支持虚拟用户、宽带限制等功能

本文所有 vsftpd 相关的操作和配置都是基于 `Ubuntu`, 其他 Linux 平台可能会有些许差异，请自行甄别

## vsftpd 安装 和 使用

```bash
# 安装
sudo apt-get -y update && sudo apt-get install -y vsftpd
# 验证
vsftpd -v
# 使用 {启动 | 停止 | 重启 | 重载配置 | 查看运行状态}
sudo service vsftpd {start|stop|restart|reload|status}
# 访问
浏览器 ftp://ip
命令行 ftp ip
```

## vsftpd 配置

Vsftpd 服务器的安装和使用都很简单，比较难的是它相对繁杂的配置，因此我这里给出了 [《一份超级详细的 vsfptd 配置介绍》](Tools/vsftpd-complete-config.md)，如果用户有需要可以根据需要自行学习和配置。

这里只给出一份常用的配置文件以供参考，实现下面的效果

- 允许 匿名用户 `ftp/anonymous` 下载文件，文件目录是 `/srv/ftp`
- 允许 白名单用户 上传 / 下载 / 删除文件，文件目录是该 Linux 用户的主目录
  - 白名单用户：配置文件 `/etc/user_list` 中的用户，一行一个
- 禁止所有用户切换到上级目录

配置步骤如下

1. 使用下面的内容替换 vsftpd 配置文件 `/etc/vsftpd.conf`

    ```bash
    # 本配置文件效果：
    #   允许 匿名用户 (ftp/anonymous) 下载文件
    #   允许 白名单用户『配置文件 /etc/user_list 中的用户』 上传 / 下载 / 删除文件
    #   禁止所有用户切换到上级目录
    #
    #   FTP 服务器使用 PASV 模式
    #       连接建立端口是 21
    #       数据传输端口是 20
    #       为数据传输分配的最小端口是 61000
    #       为数据传输分配的最大端口是 62000
    #   Tips:
    #       此配置中被注释掉的配置项通常表示其默认值是本配置文件需要的值
    #   使用须知
    #       1. 必须通过操作系统或者云服务提供商的界面上配置防火墙，开放这些端口
    #       2. 必须创建用户白名单文件：/etc/user_list, 将允许登录 FTP 服务器的用户名写入该文件，一行一个
    #       3. 如果出现客户端登录成功，ls 命令卡死的情况，只需要配置 pasv_address=${服务器公网 IP}

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

2. 重启 FTP 服务器 `sudo service vsftpd restart`
3. 配置防火墙，开放相关端口 20、21、61000-62000

    如果是云服务器，切记不仅要在操作系统层面配置防火墙，还要在云服务器提供商提供的操作界面上配置安全组等类似防火墙的规则
4. 添加 FTP 用户：这里以添加 `ftpuser` 为例

    ```bash
    # 添加一个不允许登录 Linux 系统的 Shell
    [ -z "$(grep ^/sbin/nologin$ /etc/shells)" ] && echo /sbin/nologin | sudo tee -a /etc/shells

    # 添加一个不允许登录 Linux 系统的 Linux 用户 [ftpuser], 其主目录是 /home/ftpuser
    sudo useradd -m -d /home/ftpuser ftpuser -s /sbin/nologin
    # 设置其密码 [ftpuser_password]
    sudo echo ftpuser:ftpuser_password|chpasswd
    ```

5. 将 匿名用户 `ftp/anonymous` 和 白名单用户 `ftpuser` 加入白名单

    编辑 用户白名单文件 `/etc/user_list`, 将允许登录的用户名写入该文件，一行一个

    ```bash
    ftp
    anonymous
    ftpuser
    ```

## FTP 的工作模式

FTP 协议有两种工作方式

- 主动式 (PORT)
    1. 建立命令链路：客户端向服务器的 FTP 端口（默认是 21）发送连接请求，服务器接受连接，建立一条命令链路。
    2. 建立数据链路：当需要传送数据时，客户端打开 xxx 端口，并使用 PORT 命令，通过命令链路通知服务器，服务器使用 20 端口向客户端的 xxx 端口发送连接请求，从而建立一条数据链路来传输数据
- 被动式 (PASV)
    1. 建立命令链路：客户端向服务器的 FTP 端口（默认是 21）发送连接请求，服务器接受连接，建立一条命令链路。
    2. 建立数据链路：当需要传送数据时，服务器打开 xxx 端口，并使用 PASV 命令，通过命令链路通知客户端，客户端向服务器的 xxx 端口发送连接请求，从而建立一条数据链路来传输数据

[vsftpd]:https://security.appspot.com/vsftpd.html