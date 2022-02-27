# Windows Skills

## 快捷键

| 快捷键 | 功能           |
| ------ | -------------- |
| Win+R  | 运行           |
| Win+E  | 文件资源管理器 |

## 开始菜单命令

```bash
# 打开注册表编辑器
regedit
# 关闭或开启 windows 功能
turn windows features on or off
```

## 常用命令

```bash
# 刷新 DNS
ipconfig /flushdns
```

## 奇技淫巧

### 查看文件被哪个进程占用

资源监视器 -> CPU -> 在“关联的句柄”右侧搜索框中输入被占用文件的名称

### Win10 家庭中文版 安装 Hyper-V 虚拟机

1. 创建文件 `Hyper-V.cmd`, 输入以下内容并保存

    ```cmd
    pushd "%~dp0"

    dir /b %SystemRoot%\servicing\Packages\*Hyper-V*.mum >hyper-v.txt

    for /f %%i in ('findstr /i . hyper-v.txt 2^>nul') do dism /online /norestart /add-package:"%SystemRoot%\servicing\Packages\%%i"

    del hyper-v.txt

    Dism /online /enable-feature /featurename:Microsoft-Hyper-V-All /LimitAccess /ALL
    ```

2. 保存文件并以管理员身份运行（鼠标右键 -> 以管理员身份运行）
    ![Hyper-V][]
3. 如图，安装完毕后，会提示重启计算机，按 Y 重启即可。
4. 重启后，通过开始菜单搜索 Hyper-V 管理器就可以打开啦。

## 系统设置

### 创建 中文名称 + 英文路径 的文件夹

通常，我们创建的文件夹都会使用中文名称，但有时候必须是英文路径（游戏、编程软件的安装目录)。而 windows 系统自带的 下载、文档等文件夹就满足这样的需求，因此我们也可以创建一个类似的文件夹。
这里，我以创建一个"软件"文件夹为例，使其显示中文名称，而使用英文路径"Softwares"

1. 在目标位置创建目标文件夹，使用英文名称，此处是"Software"
    ![folder-name-1][]
2. 右键属性，选择自定义，选择更改图标（为了使该文件夹变得特殊)，选择喜欢的图标并确定
    ![folder-name-2][]
3. 此时会看到文件夹的图标已经变化，在文件夹地址栏输入该文件夹的路径 +”/desktop.ini", 回车确定
    ![folder-name-3][]
4. 这时候会打开"desktop.ini"这个文件，添加一行`LocalizedResourceName= 中文名称`
    ![folder-name-4][]
5. 保存并重启电脑，会发现该文件夹已经变成中文名称，进入该文件夹，点击地址栏，会发现其路径是英文名称。
    ![folder-name-5][]

### 隐藏文件资源管理器侧边栏的资源文件夹

1. 打开注册表 `regedit`
2. 在注册表地址栏输入相应文件夹的路径

    ```bash
    # 音乐
    HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\FolderDescriptions\{a0c69a99-21c8-4671-8703-7934162fcf1d}\PropertyBag
    # 视频
    HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\FolderDescriptions\{35286a68-3c57-41a1-bbb1-0eae73d76c95}\PropertyBag

    # 桌面
    HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\FolderDescriptions\{B4BFCC3A-DB2C-424C-B029-7FE99A87C641}\PropertyBag
    # 下载
    HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\FolderDescriptions\{7d83ee9b-2244-4e70-b1f5-5393042af1e4}\PropertyBag
    # 文档
    HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\FolderDescriptions\{f42ee2d3-909f-4907-8871-4c22fc0bf756}\PropertyBag
    # 图片
    HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\FolderDescriptions\{0ddd015d-b06c-45d5-8c4c-f59713854639}\PropertyBag
    ```

3. **双击**右侧属性列表中的注册表项 **`ThisPCPolicy`**, 将 `Show` 改成 `Hide`

### 隐藏文件资源管理器侧边栏的 3D 对象文件夹

删除下面两个注册表项

```bash
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\MyComputer\NameSpace\{0DB7E03F-FC29-4DC6-9020-FF41B59E513A}
HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\Explorer\MyComputer\NameSpace\{0DB7E03F-FC29-4DC6-9020-FF41B59E513A}
```

## 其他软件

### UltraISO

制作 U 盘启动盘：`打开 -> 选择 ISO -> 启动 -> 写入硬盘映像`

### TeamViewer

1. 卸载并清除数据：`C:\Program Files (x86)\TeamViewer` & `%APPDATA%\TeamViewer`
2. 修改 MAC 地址：选择“值”, 并随便填写一个 MAC 地址 (001122334455)

    ```bash
    网络和共享中心 -> 更改适配器选项 -> 以太网 -> 鼠标右键 -> 属性 -> 配置 -> 高级 -> Network Address
    ```

3. 安装 TeamViewer: 选择个人用途
4. 修改 MAC 地址：选择不存在

[Hyper-V]:../images/windows/Hyper-V.webp
[folder-name-1]:../images/windows/folder-name-1.webp
[folder-name-2]:../images/windows/folder-name-2.webp
[folder-name-3]:../images/windows/folder-name-3.webp
[folder-name-4]:../images/windows/folder-name-4.webp
[folder-name-5]:../images/windows/folder-name-5.webp