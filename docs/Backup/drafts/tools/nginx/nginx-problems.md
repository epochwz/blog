# Nginx 常见问题

---

## 403 Forbidden

**问题描述**：安装 Nginx 后，浏览器访问时出现 `403 Forbidden`

**问题原因**：Nginx 工作用户的权限问题

**解决方法**：`sed -i "/^\(# \)*user/c user $USER;" $NGINX_HOME/conf/nginx.conf`

**排查过程**

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

5. 重载配置文件 `nginx -s reload`