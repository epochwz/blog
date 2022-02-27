# Nginx

Nginx 是一款轻量级 Web 服务器、反向代理服务器，具有高稳定、高性能、资源占用少、功能丰富、模块化结构、支持热部署等特点，支持以下功能

- 直接支持 Rails 和 PHP 的程序
- 可作为 HTTP 反向代理服务器
- 作为负载均衡服务器
- 作为邮件代理服务器
- 帮助实现前端动静分离

## Nginx 安装

这里以安装 `Nginx 1.12.1` 为例，步骤如下

1. 下载并安装

    ```bash
    # 安装 Nginx 依赖
    sudo apt-get install -y gcc make openssl libssl-dev zlib1g-dev libpcre3 libpcre3-dev

    # 版本名称
    VERSION_NAME=nginx-1.12.1
    # 安装包名称
    ZIP_NAME=$VERSION_NAME.tar.gz
    # 安装包下载链接
    ZIP_URL=http://nginx.org/download/$ZIP_NAME

    # 下载并解压
    wget -c $ZIP_URL && tar -zxvf $ZIP_NAME

    # 进入解压目录：预编译 && 编译 & 安装
    cd $VERSION_NAME && ./configure && make && sudo make install
    ```

2. 配置环境变量

    ```bash
    # 配置环境变量
    echo "export NGINX_HOME=/usr/local/nginx" | sudo tee -a /etc/profile
    # 通常将 Nginx 命令的路径加入 PATH 就可以执行 nginx 命令了
    #   但是由于 Linux 只有 root 用户可以使用 1024 以下的端口
    #       普通用户将无法执行 nginx/sudo nginx 命令，因此不使用这种做法
    # echo "export PATH=\$NGINX_HOME/sbin" | sudo tee -a /etc/profile
    echo "alias nginx=\$NGINX_HOME/sbin/nginx" | sudo tee -a /etc/profile
    # 重载环境变量配置文件使之生效
    source /etc/profile
    ```

3. 验证和启动

    ```bash
    # 验证
    nginx -v
    # 启动
    nginx
    ```

4. 配置防火墙，开放相关端口 80、443

    如果是云服务器，切记不仅要在操作系统层面配置防火墙，还要在云服务器提供商提供的操作界面上配置安全组等类似防火墙的规则

5. 浏览器访问 Nginx 首页：`http://IP`

## Nginx 使用

```bash
nginx -v            # 验证 nginx 版本号
nginx -t            # 验证 nginx 配置文件是否正确

nginx               # 启动
nginx -s stop/quit  # 停止
nginx -s reload     # 重载配置文件

# 平滑重启
kill -HUP $(pidof nginx)
  #   1. 如果 Nginx 接收到 -HUP 信号，则检测配置文件是否有更新
  #   2. 如果有更新，则应用新的配置文件，并运行新的工作进程
  #   3. 如果此时尚有客户端请求未响应 ，则继续维持连接，提供服务
  #   4. 如果处理完当前所有客户端连接，则从容关闭旧的工作进程，并启动新的工作进程
  #   5. 如果新的配置文件应用失败，将使用旧的配置文件继续工作
```

## Nginx 域名转发配置

**域名转发**就是监听 `域名 + 端口` 上的请求，然后将请求转发到 `目标目录` 或 `域名 + 端口`上

**域名转发配置步骤如下**

1. 新建一个专门存放域名配置文件的目录 `$NGINX_HOME/conf/vhost`, 避免 Nginx 主配置文件臃肿

    ```bash
    mkdir -p $NGINX_HOME/conf/vhost
    ```

2. 在**主配置文件** `$NGINX_HOME/conf/nginx.conf` 中导入`vhost`下的所有域名配置文件

    ```bash
    http{
        # ... 默认内容 ...
        include vhost/*.conf
        # ... 默认内容 ...
    }
    ```

3. 在 `vhost` 目录下添加自定义的域名配置文件 `$domain.conf`, 示例如下

    - 转发到 **域名 + 端口**

        ```bash
        # $NGINX_HOME/conf/vhost/www.epoch.fun.conf
        server{
            # 监听的端口
            listen  80;
            # 监听的域名
            server_name www.epoch.fun;

            # 匹配转发规则
            location / {
                # 转发到 域名 + 端口
                proxy_pass http://localhost:8080;
            }
        }
        ```

    - 转发到 **目录**

        ```bash
        # $NGINX_HOME/conf/vhost/ftp.epoch.fun.conf

        server {
            listen 80;
            server_name ftp.epoch.fun;
            # 禁止自动创建文件目录索引『只能通过 URL 访问单个文件』
            autoindex off;

            # 匹配转发规则
            location / {
                # 转发到 目标目录
                root /home/ftp;
            }
        }
        ```

## Nginx 常见错误

### 403 Forbidden

问题描述：安装 Nginx 后，浏览器访问出现 `403 Forbidden`

问题原因：Nginx 工作用户的权限问题

解决方法：`sed -i "/^\(# \)*user/c user $USER;" $NGINX_HOME/conf/nginx.conf`

排查过程

1. 查看 Nginx 错误日志 `$NGINX_HOME/logs/error.log`, 初步确定是权限问题

    ```bash
    "$NGINX_HOME/html/index.html" is forbidden (13: Permission denied)
    ```

2. 查看 `html` 文件夹及其子文件的权限，全部用户都有读权限，可以确定不是文件权限的问题，那么应该是用户权限的问题

    ```bash
    drwxr-xr-x  2 root   root 4096 Mar 27 22:18 html/
    -rw-r--r--  1 root root  612 Mar 27 22:18 index.html
    ```

3. 查看 nginx 进程，可以看到 nginx 的工作用户是 `nobody`, 而 nginx 目录所属用户是 `root:root`

    ```bash
    root     25862     1  0 22:11 ?        00:00:00 nginx: master process /root/softwares/nginx-1.12.1/sbin/nginx
    nobody   25863 25862  0 22:11 ?        00:00:00 nginx: worker process
    ```

4. 因此需要修改 nginx 的工作用户

    ```bash
    user root; # 如果是其他用户安装的 Nginx 则使用其他用户即可
    worker_processes  1;
    ```

5. 重启 `nginx -s reload`