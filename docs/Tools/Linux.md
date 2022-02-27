# Linux Skills

## 远程虚拟终端 Screen

```bash
# 安装
sudo yum install screen       # CentOS
sudo apt-get install screen   # Ubuntu

# 创建 screen 虚拟会话
screen -S sessionName

# 执行耗时任务时可以关闭该终端，ssh 重新连接后可恢复会话

# 恢复 screen 虚拟会话
screen -r sessionName

# 退出 screen 虚拟会话
exit

# 查看所有 screen 会话
screen -ls

# 删除会话
screen -wipe sesssionName
```

## Ubuntu 死机解决方法

```bash
1. Ctrl+Alt+F1 进入终端界面，top 显示进程 (M/ 内存排序、P/CPU 排序)，kill -9 可能造成死机的进程
2. Ctrl+Alt+F1 进入终端界面，sudo restart lightdm 或者 sudo pkill Xorg 重启桌面系统
3. 终极大杀器 -- 魔法键：(Fn/Ctrl)+Alt+SysRq+[R/1s]+[E/30s]+[I/10s]+[S/5s]+[U/5s]+[B]
```

## 其他小技巧

### 制作 U 盘启动盘

`sudo dd if=*.iso of=usbDevice`