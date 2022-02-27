# Docker

## 安装

- **Ubuntu**

  ```bash
  wget -qO-  https://get.docker.com | bash
  ```

- **CentOS**

  ```bash
  yum -y update && yum install -y docker
  service docker start/stop/restart
  ```

## 镜像管理

- 查找和下载

  ```bash
  docker search xxx
  docker pull xxx
  docker images
  docker save xxx > /path/to/filename.tar.gz
  docker load < /path/to/filename.tar.gz
  docker rmi xxx
  ```

## 容器管理

启动镜像会创建出一个运行状态的容器

```bash
docker run -it -p srcPort:desPort -p 9000:8080 -v srcPath:desPath --privileged --name {container-name} <image-name> bash

docker run -it -p 9000:8080 -v /root/projects/seckill:/root/seckill --privileged --name myjava java bash

docker pause    <container-name>    # 暂停容器
docker unpause  <container-name>    # 恢复容器
docker stop     <container-name>    # 停止容器
docker start -i <container-name>    # 启动容器
docker rm       <container-name>    # 删除容器
docker run      <container-name>    # 创建容器
docker ps -a                        # 查看容器
```

## Docker Swarm

```bash
# 创建集群
docker swarm init --listen-addr ip:port (管理者节点) --advertise-addr ip <broadcast-address>
# 加入集群 (获取加入命令)
docker swarm join-token <manager|worker>
# 查看集群节点
docker node ls
# 创建共享网络
docker network create -d overlay --attachable <network-name>
# 退出集群
docker swarm leave {--force}
# 删除集群节点
docker node demote  # 降级
service docker stop # 停止
docker node rm <id> # 删除
```

```bash
echo "OPTIONS='-Htcp://0.0.0.0:2375 -H unix://var/run/docker.sock'" >> /etc/sysctl.conf

docker pull portainer/portainer
```

```bash
docker pull     # 拉取镜像
docker build    # 构建镜像
docker images   # 列出镜像
docker run      # 运行容器
docker ps       # 列出容器
docker rm       # 删除容器
docker rmi      # 删除镜像
docker cp       # 复制文件 (宿主机 <-> 虚拟机)
docker commit   # 保存容器 -> 镜像
```

## Dockerfile

### Getting Started

1. 编写 Dockerfile 文件 `vim Dockerfile`

   ```dockerfile
   FROM alpine:latest
   MAINTAINER epoch
   CMD echo "Hello Docker"
   ```
2. 构建镜像

   ```bash
   docker build -t hello-world .
   ```

3. 运行镜像

   ```bash
   docker run hello-world
   ```

### Advanced

1. 编写 Dockerfile 文件 `vim Dockerfile`

   ```dockerfile
   FROM ubuntu
   MAINTAINER epoch
   RUN sed -i "s/\(archive\|security\).ubuntu.com/mirrors.aliyun.com/g" /etc/apt/sources.list
   RUN apt-get update -y
   RUN apt-get install -y nginx
   COPY index.html /var/www/html
   ENTRYPOINT ["/usr/sbin/nginx","-g","daemon off;"]
   EXPOSE 80
   ```
2. 构建镜像

   ```bash
   docker build -t nginx .
   ```

3. 运行镜像

   ```bash
   docker run -d -p 80:80 --name nginx nginx
   ```

### Grammar

```dockerfile
FROM    拉取基础镜像
RUN     执行命令
ADD     添加文件
COPY    复制文件
CMD     执行命令
EXPOSE  暴露端口
WORKDIR     指定运行命令的路径
MAINTAINER  维护者
ENTRYPOINT  容器入口
ENV         设置环境变量;
USER        指定用户
VOLUME      mount point
```

镜像分层

- Dockerfile 的每一行都产生一个新层，只读
- Dockerfile 构建出来的镜像一旦运行，便产生一个新层 (容器层)，可读可写

## Volume

数据卷：提供独立于容器的持久化存储

```bash
# 映射到随机目录
docker run -d -v /usr/share/nginx/html --name nginx nginx
# 映射到指定目录
docker run -d -v /root/html:/usr/share/nginx/html --name nginx nginx
# 映射到数据容器
docker create -v /path/to/host/data:/var/data --name data_container ubuntu
docker run -it --volumes-from data_container ubuntu /bin/bash
```

## term

```bash
daemon      # 守护进程
client      # 客户端
host        # 宿主机
registry    # 仓库
image       # 镜像
container   # 容器
```

## registry

```bash
docker search <image>
docker pull <image>
docker login
docker push <username>/<image>
```

## Docker Compose

```bash
sudo curl -L "https://github.com/docker/compose/releases/download/1.25.5/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose && chmod +x /usr/local/bin/docker-compose
```

```bash
build # 本地创建镜像
command # 覆盖默认命令
depends_on  # 指示容器依赖关系
ports       # 暴露端口
volumes     # 挂载目录
images      # 拉取镜像

up      # 启动
stop    # 停止
rm      # 删除服务中的全部容器
logs    # 观察各个容器的日志
ps      # 列出服务相关的容器
```