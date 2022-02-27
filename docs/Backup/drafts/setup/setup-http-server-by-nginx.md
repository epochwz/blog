# 使用 Nginx 搭建 HTTP 文件服务器

---

> [nginx] 是一款轻量级的 Web 服务器，具有高稳定、高性能、模块化、资源占用少、支持热部署等特点

Nginx 支持但不限于以下常用功能

- 直接支持 Rails 和 PHP 的程序
- 作为反向代理服务器
- 作为负载均衡服务器
- 作为邮件代理服务器
- 实现前后端动静分离

本文就简单介绍一下如何使用 `nginx` 的反向代理功能，在 `linux` 上搭建一个 HTTP 文件服务器

- 系统：`Ubuntu 19.04 x64`
- 软件：`Nginx 1.16.1`

如果在安装和使用的过程中遇到任何错误，请参考 [Nginx 常见问题][nginx-problems]

## 源码安装

1. 安装依赖

   ```bash
   sudo apt-get install -y gcc make openssl libssl-dev zlib1g-dev libpcre3 libpcre3-dev
   ```

2. 下载并解压

   ```bash
   wget -c http://nginx.org/download/nginx-1.16.1.tar.gz && tar -zxvf nginx-1.16.1.tar.gz
   ```

3. 进入解压目录并执行预编译
   ```bash
   cd nginx-1.16.1 && ./configure
   ```
4. 编译 & 安装

   ```bash
   make && sudo make install
   ```

5. 配置环境变量

   ```bash
   echo "export NGINX_HOME=/usr/local/nginx" | sudo tee -a /etc/profile
   echo "export PATH=\$NGINX_HOME/sbin:\$PATH" | sudo tee -a /etc/profile
   ```

   *温馨提示*：因为 Linux 只有 root 用户可以使用 1024 以下的端口，所以普通用户将无法执行 `nginx / sudo nginx` 命令。

   如果需要让普通用户执行 `nginx` 命令，可以取巧地使用 `alias`

   ```bash
   echo "alias nginx='sudo \$NGINX_HOME/sbin/nginx'" | sudo tee -a /etc/profile
   ```

6. 重载环境变量
   ```bash
   source /etc/profile
   ```

7. 验证和启动

   ```bash
   # 验证
   nginx -v
   # 启动
   nginx
   ```

8. 配置防火墙，开放相关端口 `HTTP: 80`、`HTTPS: 443`
   - 如果是本地机器，需要在操作系统层面配置防火墙
   - 如果是云服务器，通常只需要在云服务器提供商提供的操作界面上配置安全组等类似防火墙的规则
9. 使用浏览器访问 Nginx 首页：`http://ip`，只要出现欢迎页面，则说明安装成功了

## 请求转发

**请求转发** 就是指 Nginx 监听 `主机 + 端口` 上的请求，然后将请求转发到另一个 `主机 + 端口` 或 `文件目录` 上

本文的目的是要搭建一个 HTTP 文件服务器，因此只需要将特定请求转发到期望的文件目录上即可

**示例**：监听本机端口 `8888`, 并将请求转发到目录 `/srv/ftp/` 上

**效果**：当请求 `http://ip:8888/` 时，会返回 `/srv/ftp/` 的文件列表

1. 配置防火墙，开放端口 `8888`
2. 在 Nginx 配置文件 `$NGINX_HOME/conf/nginx.conf` 中添加下面的内容

    ```bash
    http{
      server{
        ......
      }
      ......
      # ==================== 新增的内容 开始 ==================== 
      # 新增一个 server 节点，用于配置监听的主机和端口，以及对应的请求转发规则
      server {
        listen 8888;
        server_name localhost;
        autoindex on; # 自动列出所有文件和目录列表
        default_type 'text/html';
        charset utf-8;
        
        # 请求 URL 的匹配规则
        location / {
          # 将请求转发到 目标目录
          root /srv/ftp;
        }
      }
      # ==================== 新增的内容 结束 ==================== 
      # another virtual host using mix of IP-, name-, and port-based configuration
      ......
    }
    ```
3. 重载配置文件

    ```bash
    nginx -s reload 
    ```

### 域名解析

- 域名解析：建立 **Domain--IP** 的映射关系，使得在浏览器中输入域名时能够访问到真正的服务器
- 真实的域名解析：通常由域名提供商提供域名解析服务
- 虚拟的域名解析：修改 `hosts` 配置文件 `/etc/hosts` 的内容，这也是使用 hosts 翻墙的原理

   ```
   # sample
   127.0.0.1 domain
   # example
   127.0.0.1 file.epoch.fun
   ```

## 日常维护

```bash
nginx -v            # 验证 nginx 版本号
nginx -t            # 验证 nginx 配置文件是否正确

nginx               # 启动
nginx -s stop/quit  # 停止
nginx -s reload     # 重载配置文件
```

[nginx]: http://nginx.org/en/download.html
[nginx-problems]:/docs/tools/nginx/nginx-problems.md
