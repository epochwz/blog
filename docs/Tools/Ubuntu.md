# Ubuntu 软件安装

本文汇总了 [Ubuntu][] 上常用软件的安装方法，以便随时查看；同时也基于 `Ubuntu 16.04` 编写了简单的 [Shell Scripts][]，用于快速安装、切换版本。

**温馨提示：** 安装下文任意软件之前，必须先执行**一次**以下命令，初始化基本路径，也可以根据自己的习惯更改

```bash
# 软件安装包路径
export PACKAGE_PATH=$HOME/.packages && mkdir -p $PACKAGE_PATH
# 软件安装路径
export SOFTWARE_PATH=$HOME/softwares && mkdir -p $SOFTWARE_PATH
# 软件源码路径
export SRCCODE_PATH=$BASE_PATH/.src && mkdir -p $SRCCODE_PATH
# 环境变量文件路径
export ENV_FILE=$HOME/.bash_aliases && touch $ENV_FILE
```

**温馨提示：** 想偷懒的话，可以将以下任意软件的安装步骤复制并粘贴到一个脚本文件 `xxx.sh` 中，并使用 `bash xxx.sh` 执行，不必一行一行执行

## 更换 APT 软件源

- [阿里云开源镜像站](https://opsx.alibaba.com/mirror?lang=zh-CN)
- Ubuntu 软件源配置文件：`/etc/apt/source.list`
- Ubuntu 第三方软件源配置文件目录：`/etc/apt/source.list.d`

示例：将 `Ubuntu 16.04` 软件源更换成阿里云的软件源

1. 备份旧的软件源

    ```bash
    sudo mv /etc/apt/source.list /etc/apt/source.list.bak
    ```

2. 更换新的软件源：将软件源配置文件 `/etc/apt/source.list` 的内容替换如下

    ```bash
    deb http://mirrors.aliyun.com/ubuntu/ xenial main
    deb-src http://mirrors.aliyun.com/ubuntu/ xenial main

    deb http://mirrors.aliyun.com/ubuntu/ xenial-updates main
    deb-src http://mirrors.aliyun.com/ubuntu/ xenial-updates main

    deb http://mirrors.aliyun.com/ubuntu/ xenial universe
    deb-src http://mirrors.aliyun.com/ubuntu/ xenial universe
    deb http://mirrors.aliyun.com/ubuntu/ xenial-updates universe
    deb-src http://mirrors.aliyun.com/ubuntu/ xenial-updates universe

    deb http://mirrors.aliyun.com/ubuntu/ xenial-security main
    deb-src http://mirrors.aliyun.com/ubuntu/ xenial-security main
    deb http://mirrors.aliyun.com/ubuntu/ xenial-security universe
    deb-src http://mirrors.aliyun.com/ubuntu/ xenial-security universe
    ```

3. 更新软件源缓存

    ```bash
    sudo apt-get update -y
    ```

## 安装 Oracle JDK

### 通过 PPA 安装

**温馨提示：** 此安装过程需要手动操作接受 Oracle 的协议

```bash
# JDK 1.8

# 主版本号
MAJOR_VERSION=8

# 安装 GPG Keys
gpg --keyserver keyserver.ubuntu.com --recv-keys C2518248EEA14886
gpg -a --export C2518248EEA14886 | sudo apt-key add -

# 安装 PPA
sudo apt-get install -y software-properties-common
sudo add-apt-repository -y ppa:webupd8team/java

# 安装
sudo apt-get update -y && sudo apt-get install -y oracle-java$MAJOR_VERSION-installer

# 验证
javac -version && echo && java -version
```

### 通过 压缩包 安装

```bash
# JDK 1.8

# 版本签名
VERSION_VERIFY=8u201-b09/42970487e3af4f5aa5bca3f542482c60
# 版本名称
VERSION_NAME=jdk1.8.0_201
# 压缩包名称
ZIP_NAME=jdk-8u201-linux-x64.tar.gz

# 压缩包下载链接前缀
ZIP_BASE_URL=https://download.oracle.com/otn-pub/java/jdk
# 压缩包下载链接
ZIP_URL=$ZIP_BASE_URL/$VERSION_VERIFY/$ZIP_NAME
# 压缩包下载位置
ZIP_PATH=$PACKAGE_PATH/$ZIP_NAME
# 软件安装路径
JAVA_HOME=$SOFTWARE_PATH/$VERSION_NAME

# 免验证下载 Oracle JDK 安装包
wget -c --no-check-certificate --no-cookies --header "Cookie: oraclelicense=accept-securebackup-cookie" $ZIP_URL -O $ZIP_PATH
# 解压
tar -zxvf $ZIP_PATH -C $SOFTWARE_PATH

# 配置环境变量
echo "export JAVA_HOME=$JAVA_HOME" >> $ENV_FILE
echo "export PATH=\$JAVA_HOME/bin:\$PATH" >> $ENV_FILE
# 重载环境变量配置文件使之生效
source $ENV_FILE

# 验证
javac -version && echo && java -version
```

## 安装 Maven

```bash
# Maven 3.5.0

# 完整版本号
VERSION=3.5.0

# 主版本号
MAJOR_VERSION=$(echo $VERSION | cut -f 1 -d ".")
# 版本名称
VERSION_NAME=apache-maven-$VERSION
# 压缩包名称
ZIP_NAME=$VERSION_NAME-bin.tar.gz
# 压缩包下载链接前缀
ZIP_BASE_URL=https://archive.apache.org/dist/maven
# 压缩包下载链接
ZIP_URL=$ZIP_BASE_URL/maven-$MAJOR_VERSION/$VERSION/binaries/$ZIP_NAME
# 压缩包下载路径
ZIP_PATH=$PACKAGE_PATH/$ZIP_NAME
# 软件安装路径
MAVEN_HOME=$SOFTWARE_PATH/$VERSION_NAME

# 下载并解压
wget -c $ZIP_URL -O $ZIP_PATH && tar -zxvf $ZIP_PATH -C $SOFTWARE_PATH

# 配置环境变量
echo "export MAVEN_HOME=$MAVEN_HOME" >> $ENV_FILE
echo "export PATH=\${MAVEN_HOME}/bin:\$PATH" >> $ENV_FILE
# 重载环境变量配置文件使之生效
source $ENV_FILE

# 验证
mvn -v
```

## 安装 Tomcat

```bash
# Tomcat 8.5.23

# 完整版本号
VERSION=8.5.23

# 主版本号
MAJOR_VERSION=$(echo $VERSION | cut -f 1 -d ".")
# 版本名称
VERSION_NAME=apache-tomcat-$VERSION
# 压缩包名称
ZIP_NAME=$VERSION_NAME.tar.gz
# 压缩包下载链接前缀
ZIP_BASE_URL=https://archive.apache.org/dist/tomcat
# 压缩包下载链接
ZIP_URL=$ZIP_BASE_URL/tomcat-$MAJOR_VERSION/v$VERSION/bin/$ZIP_NAME
# 压缩包下载路径
ZIP_PATH=$PACKAGE_PATH/$ZIP_NAME
# 软件安装路径
CATALINA_HOME=$SOFTWARE_PATH/$VERSION_NAME

# 下载并解压
wget -c $ZIP_URL -O $ZIP_PATH && tar -zxvf $ZIP_PATH -C $SOFTWARE_PATH

# 配置环境变量
echo "export CATALINA_HOME=$CATALINA_HOME" >> $ENV_FILE
echo "export PATH=\$CATALINA_HOME/bin:\$PATH" >> $ENV_FILE
# 重载环境变量配置文件使之生效
source $ENV_FILE

# 启动
startup.sh

# 配置防火墙，开放相关端口 (eg. 8080)
# 浏览器验证：http://localhost:8080 | http://ip:8080

# 配置 UTF-8 字符集
#   1. vim $CATALINA_HOME/conf/server.xml
#   2. 修改 8080 节点的内容为：<Connector port="8080" ...... URIEncoding="UTF-8">
```

## 安装 Nginx

```bash
# 版本名称
VERSION_NAME=nginx-1.12.1

# 安装包名称
ZIP_NAME=$VERSION_NAME.tar.gz
# 安装包下载链接
ZIP_URL=http://nginx.org/download/$ZIP_NAME
# 安装包下载路径
ZIP_PATH=$PACKAGE_PATH/$ZIP_NAME
# 安装目录
NGINX_HOME=$SOFTWARE_PATH/$VERSION_NAME

# 安装 Nginx 依赖
sudo apt-get install -y gcc make openssl libssl-dev zlib1g-dev libpcre3 libpcre3-dev

# 下载并解压
wget -c $ZIP_URL -O $ZIP_PATH && tar -zxvf $ZIP_PATH -C $SRCCODE_PATH

# 进入解压目录
cd $SRCCODE_PATH/$VERSION_NAME
# 预编译：指定安装目录，默认安装路径 usr/local/nginx
./configure --prefix=$NGINX_HOME
# 编译 & 安装
make && make install

# 配置环境变量
echo "export NGINX_HOME=$NGINX_HOME" >> $ENV_FILE
echo "export PATH=\$NGINX_HOME/sbin:\$PATH" >> $ENV_FILE
# 重载环境变量配置文件使之生效
source $ENV_FILE

# 验证
nginx -v
# 启动
nginx

# 解决 403 Forbidden
sed -i "/^#user/c user $USER;" $NGINX_HOME/conf/nginx.conf
# 重载配置文件
nginx -s reload

# 配置防火墙，开放相关端口 (eg. 80,443)
# 浏览器访问：http://IP
```

## 安装 MySQL

更多信息参考 [MySQL APT 安装向导](https://dev.mysql.com/doc/mysql-apt-repo-quick-guide/en/)

```bash
# MySQL 5.7

# MySQL APT 配置软件安装包 版本号
VERSION=0.8.12-1

# 安装包名称
ZIP_NAME=mysql-apt-config_${VERSION}_all.deb
# 安装包下载链接
ZIP_URL=https://repo.mysql.com/$ZIP_NAME
# 安装包下载路径
ZIP_PATH=$PACKAGE_PATH/$ZIP_NAME

# 下载并安装
wget -c $ZIP_URL -O $ZIP_PATH && sudo dpkg -i $ZIP_PATH

# 安装 MySQL
sudo apt-get update -y && sudo apt-get install -y mysql-server
# 运行 交互式 - 安全配置工具
sudo mysql_secure_installation

# 验证
mysql -V

# 配置字符集
# vim /etc/mysql/conf.d/mysql.cnf
#   [mysql] 节点下添加 defautl-character-set=utf8
# vim /etc/mysql/mysql.conf.d/mysqld.cnf
#   [mysqld] 节点下添加 character-set-server=utf8
```

## 安装 Nodejs

```bash
# 完整版本号
VERSION=4.4.7
# 版本名称
VERSION_NAME=node-v$VERSION
# 压缩包名称
ZIP_NAME=$VERSION_NAME-linux-x64.tar.gz
# 压缩包下载链接前缀
ZIP_BASE_URL=https://nodejs.org/download/release/
# 压缩包下载链接
ZIP_URL=$ZIP_BASE_URL/v$VERSION/$ZIP_NAME
# 压缩包下载路径
ZIP_PATH=$PACKAGE_PATH/$ZIP_NAME
# 软件安装路径
NODE_HOME=$SOFTWARE_PATH/$VERSION_NAME

# 下载并解压
sudo wget -c $ZIP_URL -O $ZIP_PATH && sudo tar -zxvf $ZIP_PATH -C $SOFTWARE_PATH

# 配置环境变量
echo "export NODE_HOME=$NODE_HOME" >> $ENV_FILE
echo "export PATH=\$NODE_HOME/bin:\$PATH" >> $ENV_FILE
# 重载环境变量配置文件使之生效
source $ENV_FILE

# 验证
node -v
```

[Ubuntu]:https://www.ubuntu.com/download/alternative-downloads
[Shell Scripts]:https://github.com/epochwz/epochwz.github.io