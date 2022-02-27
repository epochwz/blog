# PXC

1. 安装 PXC 的 Docker 镜像

   ```bash
   docker pull percona/percona-xtradb-cluster:5.7.21
   docker tag percona/percona-xtradb-cluster:5.7.21 pxc
   docker rmi percona/percona-xtradb-cluster:5.7.21 
   ```
2. 创建 Docker 内部网络

   ```bash
   docker network create --subnet=172.11.21.0/24 net1
   # docker network ls             # 查看网络列表
   # docker network inspect net1   # 查看网络信息
   # docker network rm net1        # 删除网络
   ```
3. 创建 Docker 数据卷 (因为 PXC 无法直接使用 Docker 的目录映射)

   ```bash
   docker volume create --name v0
   docker volume create --name v1
   docker volume create --name v2
   docker volume create --name v3
   docker volume create --name v4
   ```
4. 创建 PXC 容器

   ```bash
   docker run -d --name=m0 -p 3305:3306 -v v0:/var/lib/mysql --net=net1 --ip 172.11.21.2 -e MYSQL_ROOT_PASSWORD=root -e XTRABACKUP_PASSWORD=root -e CLUSTER_NAME=PXC pxc

   docker run -d --name=m1 -p 3307:3306 -v v1:/var/lib/mysql --net=net1 --ip 172.11.21.3 -e MYSQL_ROOT_PASSWORD=root -e XTRABACKUP_PASSWORD=root -e CLUSTER_NAME=PXC -e CLUSTER_JOIN=m0 pxc

   docker run -d --name=m2 -p 3308:3306 -v v2:/var/lib/mysql --net=net1 --ip 172.11.21.4 -e MYSQL_ROOT_PASSWORD=root -e XTRABACKUP_PASSWORD=root -e CLUSTER_NAME=PXC -e CLUSTER_JOIN=m0 pxc

   docker run -d --name=m3 -p 3309:3306 -v v3:/var/lib/mysql --net=net1 --ip 172.11.21.5 -e MYSQL_ROOT_PASSWORD=root -e XTRABACKUP_PASSWORD=root -e CLUSTER_NAME=PXC -e CLUSTER_JOIN=m0 pxc

   docker run -d --name=m4 -p 3310:3306 -v v4:/var/lib/mysql --net=net1 --ip 172.11.21.6 -e MYSQL_ROOT_PASSWORD=root -e XTRABACKUP_PASSWORD=root -e CLUSTER_NAME=PXC -e CLUSTER_JOIN=m0 pxc
   ```

5. 安装 Haproxy 负载均衡

   ```bash
   docker pull haproxy
   ```
6. 创建 Haproxy 配置文件 `/root/seckill/pxc/haproxy/haproxy.cfg`
7. 创建 Haproxy 容器

   ```bash
   docker run -it -d --name h1 -p 3301:8888 -p 3302:3306 -v /root/seckill/pxc/haproxy:/usr/local/etc/haproxy --net=net1 --ip 172.11.21.17 haproxy
   docker exec -it h1 bash
   haproxy -f /usr/local/etc/haproxy/haproxy.cfg

   docker run -it -d --name h2 -p 3303:8888 -p 3304:3306 -v /root/seckill/pxc/haproxy:/usr/local/etc/haproxy --net=net1 --ip 172.11.21.21 haproxy
   docker exec -it h1 bash
   haproxy -f /usr/local/etc/haproxy/haproxy.cfg
   ```
8. 创建 Haproxy 账号

   ```sql
   CREATE USER 'haproxy'@'%' IDENTIFIED BY '';
   ```
