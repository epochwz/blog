# RXTX

## windows 安装

- 复制 rxtxParallel.dll & rxtxSerial.dll 到 $JAVA_HOME/bin/
- 复制 RXTXcomm.jar 到 $JAVA_HOME/lib/ext/

## Linux 编译安装

- 获取源码: wget http://rxtx.qbang.org/pub/rxtx/rxtx-2.1-7r2.zip
- 解压：unzip rxtx-2.1-7r2.zip
- 解决可能的错误
    - error: ‘UTS_RELEASE’ undeclared (first use in this function)
        - vim /usr/include/linux/version.h 添加 #define UTS_RELEASE "3.10.24+"
        - 3.10.24+ = `uname -r`
    - error: libtool: install: armv6l-unknown-linux-gnu/librxtxRS485.la’ is not a directory
        - vim rxtx-2.1-7r2/configure 找到 1.2*|1.3*|1.4*|1.5* 添加 1.2*|1.3*|1.4*|1.5*|1.6*|1.7*|1.8*
- sudo -i
- sh ./configure -build=`uname -r`
- make && make install
-