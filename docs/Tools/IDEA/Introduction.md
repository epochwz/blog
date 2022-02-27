# IDEA

## 基本介绍

**安装目录**

```bash
C:\Program Files\JetBrains\IntelliJ IDEA 2017.3.6
    bin # IDEA 的 执行文件目录
        idea.exe & idea64.exe   # IDEA 的 可执行文件
        idea.exe.vmoptions      # 32 位可执行文件 的 VM 配置文件（虚拟机运行参数）
        idea64.exe.vmoptions    # 64 位可执行文件 的 VM 配置文件（虚拟机运行参数）
        idea.properties         # IDEA 的 参数配置文件
    lib # IDEA 的 依赖文件目录
```

**设置目录**

```bash
C:\Users\epoch\.IntelliJIdea2017.3  # 删掉该目录之后，重启 IDEA 会自动生成全新的设置目录
    config  # 个性配置目录（定义 IDE 主要配置 和 自定义快捷键、代码模板、文件模板等个性化配置）
    system  # 系统文件目录（缓存、索引、容器产生的文件），是 IDEA 和 开发项目之间的桥梁
        LocalHistory    # 本地历史记录目录
    # 安装新版本的 IDEA 时可以选择导入的旧配置就是 config 目录
```

**参数配置** `Help -> Edit Custom Properties`

```bash
idea.config.path=${user.home}/.IntelliJIdea/config  # 设置 个性配置目录 路径
idea.system.path=${user.home}/.IntelliJIdea/system  # 设置 系统文件目录 路径
idea.plugins.path=${idea.config.path}/plugins       # 设置 系统插件目录 路径
idea.log.path=${idea.system.path}/log               # 设置 系统日志目录 路径
```

**VM 配置** `Help -> Edit Custom VM Options`

```bash
# JVM 的默认字符集依赖于所在操作系统的区域及字符集：JVM 启动时会从系统属性 file.encoding 中读取字符集
# -D            设置系统属性
# file.encoding 系统属性名称 - 文件编码
# UTF-8         文件编码名称
-Dfile.encoding=UTF-8   # 设置 IDEA 的 JVM 启动参数：默认字符集
```

## 系统设置

```bash
Appearance & Behavior       # 外观和行为
    Appearance                  # 外观
Keymap                      # 快捷键
Editor                      # 编辑区
    Font                        # 字体
    Color Scheme                # 主题
        Console Font                # 控制台字体
    File Encodings              # 文件编码
Plugins                     # 插件
Version Control             # 版本控制
Build,Execution,Deployment  # 构建、执行、部署
    Excludes                    # 编译排除：添加不进行编译的文件 / 目录
    Compiler                    # 编译器
Languages & Frameworks      # 语言和框架
Tools                       # 工具
```

## 项目管理

**编译和构建**

```bash
Build Module    编译整个模块（只编译修改过的文件）
Build Project   编译整个项目（只编译修改过的文件）
Recompile       重新编译当前文件（强制，无论是否修改过）
Rebuild Project 重新编译整个项目（强制，无论是否修改过）
```

**相关介绍**

- `Source root` 源代码目录，标记该目录下的文件是可编译的
- `Mark Directory AS Source root` 标记选中的目录作为源代码目录

## 常用配置

通过 `File -> Other Settings -> Default Settings` 进入 `IDEA Default Settings` 界面

### 修改文件编码

```bash
全部使用 UTF-8
勾选 `Transparent native-to-ascii conversion` # 转换 ASCII, 不勾选则 properties 文件的中文注释可能乱码

单个文件修改编码：点击右下角编码按钮
    Reload  使用指定编码重新打开文件，不保存新编码到文件中（重新打开后还是原来的编码）
    Convert 使用指定编码进行编码转换，保存新编码到文件中（可能产生乱码，不可逆）
```

### 修改文件模板

Editor -> File and Code Templates -> Includes -> File Header

```bash
/**
    * description
    * <p>
    * Created by epoch on ${YEAR}/${MONTH}/${DAY}.
    */
```

### 其他设置

- Tools -> Terminal: 使用 Git Shell
- Editor -> Code Style -> Java -> Wrapping and Braces -> Keep when reformatting -> 取消 Comment at first Column
- Editor -> Code Style -> Java -> Code Generation -> Comment Code -> Add a space at comment start
- 新建一个 Java 文件 -> Ctrl+Shift+Alt+L -> Optimize imports & Code cleanup

## 代码编辑

- Live Template: 自定义代码模板
- Postfix: 补全
- Alter+Enter
  - 自动创建方法
  - list replace
  - 字符串 format/builder
  - 实现接口
  - 单词拼写
  - 导包