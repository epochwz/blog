# Ubuntu

## 更换软件源

```bash
sudo sed -i "s/\(archive\|security\).ubuntu.com/mirrors.aliyun.com/g" /etc/apt/sources.list
```

## 软件安装

## 安装 Docker

```bash
# 安装
wget -qO-  https://get.docker.com | bash
# 授权
sudo usermod -aG docker epoch

# 启动
sudo service docker start
# 验证
docker version

# 加速
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": [
    "https://ngdra8b3.mirror.aliyuncs.com"
   ]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker

# Windows WSL 设置环境变量
export DOCKER_HOST=tcp://0.0.0.0:2375
```

## 防火墙

```bash
iptables -A INPUT -p tcp --dport 4567 -j DROP
iptables -A OUTPUT -p tcp --dport 4567 -j DROP
```