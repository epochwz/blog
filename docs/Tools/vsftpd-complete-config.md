# 超级详细的 Vsfptd 配置介绍

Vsfptd 功能强大的同时，超多的配置项也让人眼花缭乱，全部配置见官方文档 [VSFTPD.CONF][]

本文基于官方文档进行翻译以及自己的操作实践给出一份超级详细的 Vsfptd 配置的介绍，以供需要时随时查看。

在下文中，均以**代码 + 注释**的形式给出每个配置项的作用、可配置的值、默认值

- 如无特别说明，下文每个配置项给出的配置值都是默认值
- 如无特别说明，中文注释中的**允许**表示配置值 **`YES`**, **禁止**表示配置值 **`NO`**
- 如无特别说明，中文注释中的**是否**中的**是**表示配置值 **`YES`**, **否**表示配置值 **`NO`**

## 全局配置

```bash
# 允许 / 禁止 登录用户具有写权限
write_enable=NO
# 允许 / 禁止 登录用户下载文件『如果禁止，那么所有的文件不能下载到本地，文件夹不受影响』
download_enable=YES

# 使用系统本机时间
use_localtime=YES
# 使用 UTF-8 文件系统
utf8_filesystem=YES

# 允许 / 禁止 FTP 根目录具有 write 权限
allow_writeable_chroot=YES

# 可以通过用『户配置文件』来实现不同的用户使用不同的 vsftpd 配置
# 指定用户配置文件目录『如果配置了该选项，那么当用户登录时，系统会到该目录下寻找该用户对应的配置文件，对当前用户进行个性化配置』
user_config_dir=/etc/vsftpd/userconf
# Example: 如果有两个 FTP 用户 userA,userB, 则可以在 /etc/vsftpd/userconf 下新增文件 userA,userB 作为各自的 vsftpd 配置文件，并在其中添加个性化配置，如访问速度控制等

# 指定在执行 ls -a 时显示文件的 UID、GID(NO) 或者 用户名、用户组 (YES)
text_userdb_names=NO
# 允许 / 禁止使用 ls -R 命令『查看当前目录下的子目录中的文件』
ls_recurse_enable=NO
# 是否启用隐藏用户 / 用户组信息的功能『如果启用，则 ls -R 所看到的档案的所属用户 / 用户组 都是 ftp]
hide_ids=NO
```

## 匿名用户 (ftp/anonymous) 配置

```bash
# 允许 / 禁止 匿名用户登录
anonymous_enable=YES
# 允许 / 禁止 匿名用户免密登录
no_anon_password=NO
# 指定匿名用户登录时使用的用户名
ftp_username=ftp
# 指定匿名登录的 文件目录『不允许 具备 777 权限的目录』
anon_root=/srv/ftp

# 允许 / 禁止 匿名用户上传文件『非目录』 『如果允许，匿名用户必须对上层目录具备写权限』
anon_upload_enable=NO
# 允许 / 禁止 匿名用户新增目录『如果允许，匿名用户必须对上层目录具备写权限』
anon_mkdir_write_enable=NO
# 允许 / 禁止 匿名用户上传文件、新增目录以外的权限，如删除 / 重命名等权限
anon_other_write_enable=NO

# 是否改变匿名用户上传的文件『非目录』的所属用户
chown_uploads=NO
# 指定匿名用户上传的文件『非目录』的所属用户
chown_username=ftp
# 指定匿名用户新增 / 上传档案的 umask 值
anon_umask=077 # 对应文件权限是 700

# ??? 如果启用的话，只允许匿名用户下载可阅读文件『通常在匿名用户拥有自己上传的文件时使用』
anon_world_readable_only=YES

# 是否启用邮箱黑名单『如果启用，则匿名用户登录必须输入 Email, 如果 Email 在邮箱黑名单中，则不允许登录』
deny_email_enable=NO
# 指定邮箱黑名单文件路径
banned_email_file=/etc/vsftpd/banner_emails
```

## 本地用户配置『匿名用户以外的用户』

```bash
# 允许 / 禁止 本地用户登录
local_enable=NO
# 指定本地用户登录的 文件目录
local_root=/home/username # 默认是该 Linux 用户的主目录

# 本地用户 新增档案的 umask 值
local_umask=077 # 对应文件权限是 700
# 本地用户 上传档案的 文件权限
file_open_mode=0666
```

## 访问 / 权限控制

### 权限控制配置

```bash
# 是否禁止用户切换到文件根目录的上级目录
# YES: 禁止所有用户切换目录，仅在启用 chroot_list_enable 时允许 /etc/chroot_list 中的用户切换目录
# NO: 允许所有用户切换目录，仅在启用 chroot_list_enable 时禁止 /etc/chroot_list 中的用户切换目录
chroot_local_user=NO
# 启用 / 关闭 切换目录的 黑 / 白名单
# chroot_local_user=YES 时是白名单
# chroot_local_user=NO 时是黑名单
chroot_list_enable=NO
# 指定 切换目录的  黑 / 白名单 的文件路径
chroot_list_file=/etc/chroot_list
```

### 用户访问控制

```bash
# 全局黑名单文件 /etc/ftpusers（最高优先级配置文件）

# 启用 / 关闭 用户登录的 黑 / 白名单文件
userlist_enable=NO
# 指定 用户登录的 黑 / 白名单 文件路径
userlist_file=/etc/user_list
# 指定 userlist_file 是黑名单还是白名单
# YES: userlist_file 是黑名单，除该名单中的用户之外其他用户都能登录
# NO:  userlist_file 是白名单，除该名单中的用户之外其他用户都不能登录
userlist_deny=YES
```

### 主机访问控制

```bash
# 启用 / 关闭 tcp wrapper 主机访问控制
tcp_wrappers=YES
# 若启用，则 vsftpd 会检查 /etc/hosts.allow 和 /etc/hosts.deny 中的设置，以确定是否允许某主机访问该 FTP 服务器
# Example：开放 192.168.0.1-192.168.0.254 的主机登录权限
/etc/hosts.allow
    vsftpd:192.168.0. :allow
    all:all :deny
```

## 连接和端口配置

```bash
# 启用 / 关闭 standalone 模式『如果关闭，则 vsftpd 不以独立模式运行，受到 xientd 服务管控，功能受限制』
listen=YES
# 指定 FTP 服务器建立连接所监听的端口
listen_port=21
# 是否使用 20 端口进行数据传输
connect_from_port_20=YES
# 指定 PORT 模式下 FTP 数据连接使用的端口
ftp_data_port=20

# 是否启用 PASV 工作模式『如果不启动，则使用默认的 PORT 工作模式』
pasv_enable=YES
# 监听特定 IP 的用户的 FTP 请求，如果不设置，则监听所有 IP 的请求『该配置只在 standlone 模式下生效』
pasv_address=127.0.0.1
# PASV 模式数据连接的 最大 / 最小 端口
pasv_max_port=0
pasv_min_port=0

# 设置每个与 FTP 服务器的连接，是否以不同进程表现『如果 NO，则 ps -ef | grep vsftpd 只有一个 vsftpd 进程，否则会有多个 vsftpd 进程』
setproctitle_enable=NO
# 允许的最大连接数『该配置只在 standlone 模式下生效』
max_clients=0 # 0 表示不受限制
# 允许每个 IP 同时与 FTP 服务器建立的连接数『该配置只在 standlone 模式下生效』
max_per_ip=0 # 0 表示不受限制
```

## 超时时间设置

```bash
# 建立 FTP 连接 的超时时间
accept_timeout=60
# 建立 FTP 数据连接 的超时时间
data_connection_timeout=120
# PORT 模式下建立 数据连接 的超时时间
connect_timeout=60
# 无操作时的超时断开时间
idle_session_timeout=300
```

## 数据传输配置

```bash
# 匿名登录 的最大传输速率 (B/s)
anon_max_rate=0 # 0 代表不限速
# 本地用户 的最大传输速率 (B/s)
local_max_rate=0 # 0 代表不限速

# 是否使用 ASCII 模式上传 / 下载传输
ascii_upload_enable=NO # 此时默认以二进制模式上传 / 下载数据
# 是否使用 ASCII 模式下载上传 / 下载数据
ascii_download_enable=NO
```

## 日志文件

```bash
# 启用 / 关闭 上传下载 日志记录
xferlog_enable=YES
# 指定上传下载日志文件
xferlog_file=/var/log/vsftpd.log
# 启用 / 关闭 xferlog 标准格式
xferlog_std_format=NO

# 启用 / 关闭 请求 & 响应 日志记录『通常用于调试，启用时不能开启 xferlog_std_format]
log_ftp_protocal=NO
```

## 欢迎语设置

```bash
# 指定用户登录欢迎语
ftpd_banner=Welcom to epoch's FTP Server!
# 指定用户登录欢迎语文件『如果欢迎语内容较多，则使用该选项』
banner_file=/etc/vsftpd/banner

# 启用 / 关闭 目录说明文件『使用者第一次进入某个目录时，检查该目录是否有 .message 文件，该文件放置了该目录的说明语』
dirmessage_enable=YES
# 指定目录说明文件
message_file=.message
```

## 虚拟用户设置

```bash
# 虚拟用户使用 PAM 认证方式，设置 PAM 使用的名称
pam_service_name=/etc/pam.d/vsftpd
# 是否启用虚拟用户
guest_enable=NO
# 映射虚拟用户
guest_name=ftp
# 允许 / 禁止 虚拟用户拥有与本地用户相同的权限『如果禁止，则虚拟用户拥有与匿名用户相同的权限』
virtual_use_local_privs=NO
```

[VSFTPD.CONF]:https://security.appspot.com/vsftpd/vsftpd_conf.html