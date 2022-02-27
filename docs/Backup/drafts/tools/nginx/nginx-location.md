## 平滑重启

```bash
kill -HUP $(pidof nginx)
    # 1. 如果 Nginx 接收到 -HUP 信号，则检测配置文件是否有更新
    # 2. 如果有更新，则应用新的配置文件，并运行新的工作进程
    # 3. 如果此时尚有客户端请求未响应 ，则继续维持连接，提供服务
    # 4. 如果处理完当前所有客户端连接，则从容关闭旧的工作进程，并启动新的工作进程
    # 5. 如果新的配置文件应用失败，将使用旧的配置文件继续工作
```

## 反向代理

**反向代理** 就是监听 `主机 + 端口` 上的请求，然后将请求转发到另一个 `主机 + 端口` 或 `目标目录` 上

**配置步骤**

1. 新建一个专门存放域名配置文件的目录 `$NGINX_HOME/conf/vhost`, 避免 Nginx 主配置文件臃肿

   ```bash
   mkdir -p $NGINX_HOME/conf/vhost
   ```

2. 在 **主配置文件** `$NGINX_HOME/conf/nginx.conf` 中导入 `vhost` 下的所有域名配置文件

   ```bash
   http{
     server{
       ......
     }
     ......
     include vhost/*.conf;
     # another virtual host using mix of IP-, name-, and port-based configuration
     ......
   }
   ```

3. 在 `vhost` 目录下添加自定义的域名配置文件 `$domain.conf`

   - 示例：转发到 **域名 + 端口**

     ```bash
     # $NGINX_HOME/conf/vhost/www.epoch.fun.conf
     server{
       # 监听的端口
       listen  80;
       # 监听的域名
       server_name www.epoch.fun;
       
       # URL 匹配规则
       location / {
         # 转发到 域名 + 端口
         proxy_pass http://localhost:8080;
       }
     }
     ```

   - 示例：转发到 **目录**

     ```bash
     # $NGINX_HOME/conf/vhost/ftp.epoch.fun.conf
     server {
       listen 80;
       server_name file.epoch.fun;
       # 禁止自动创建文件目录索引 (只能通过 URL 访问单个文件)
       autoindex off;
       
       # URL 匹配规则
       location / {
         # 转发到 目标目录
         root /home/ftp;
       }
     }
     ```
