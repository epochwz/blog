# CentOS

## 防火墙

```bash
# 状态查看
firewalld-cmd --state
# 状态管理
service firewalld start
service firewalld stop
service firewalld restart
# 端口管理
firewalld-cmd --permanent --add-port=8080-8085/tcp
firewalld-cmd --reload
firewalld-cmd --permanent --remove-port=8080-8085/tcp
firewalld-cmd --reload
# 查看开启的端口
firewalld-cmd --permanent --list-ports
# 查看开启的服务
firewalld-cmd --permanent --list-services
```

## Docker

```bash
yum -y update && yum install -y docker

service docker start/stop/restart

# 镜像管理
# 查找和下载
docker search xxx
docker pull xxx
docker images
docker save xxx > /path/to/filename.tar.gz
docker load < /path/to/filename.tar.gz
docker rmi xxx
# 配置 DaoCloud 镜像仓库
```