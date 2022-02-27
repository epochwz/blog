# RPM安装包

**RPM包命名规则**

**包名**-版本号-软件发布次数.适合的Linux平台.适合的硬件平台.**rpm**

**包全名&包名**

操作的包是系统尚未安装的RPM包时使用**包全名**
操作已安装的RPM包时使用**包名**，系统在/var/lib/rpm/中的数据库中自动搜索匹配的包

**RPM包依赖**

```
树形依赖：a->b->c
环形依赖：a->b->c->a
模块依赖：依赖(某个rpm包中的)库文件，需要通过www.rpmfind.net查询并安装该rpm包
```

**RPM包查询**

```
# 查询RPM包是否已安装
$ rpm -q(query) <packageName>
# 查询所有已经安装的含关键字keyword的RPM包
$ rpm -qa(all) | grep <keyword>
# 查询系统文件所属的RPM包
$ rpm -qf <fileName>

# 查询RPM包中文件的安装位置
-l(location)
# 查询RPM包的依赖性
-R(Reference)
# 查询RPM包的信息
-i(information)
# 查询未安装包的信息
-p(package)
```

**RPM包校验**

```
# RPM包完整性校验
$ rpm -V packageName

# 查询以下项是否被改变
S    文件大小
M    文件类型或文件权限
5    文件MD5校验和
D    设备的主从代码
L    文件路径
U    文件的所有者
G    文件的所属组
T    文件的修改时间

# 文件类型
c    配置文件(config)
d    普通文档(document)
g    “鬼”文件(ghost):该RPM包不应该含有的文件
L    授权文件(License)
r    描述文件(README)
```

**RPM包文件提取**

```
$ rpm2cpio 包全名 | cpio -idv .文件绝对路径

-rpm2cpio    # 将rpm包转换成cpio格式的命令
-cpio        # 用于创建软件档案文件、从档案文件中提取文件的标准工具

$ cpio [Options] < 文件|设备
-i    copy-in模式，还原
-d    还原时自动新建目录
-v    显示还原过程
```

**本地安装**

```bash
# 安装(install)
$ rpm -ivh <full-packageName>
# 升级(Upgrade)
$ rpm -Uvh <full-packageName>
# 卸载(erase)
$ rpm -e <packageName>

# 显示详细信息
-v(verbose)
# 显示安装进度
-h(hash)
# 不检查依赖性，通常禁止使用
--nodeps
```

**yum在线安装**

**yum：**使用官方服务器存放所有rpm软件包，自动解决依赖性问题

```
# 列出yum源所有可用RPM包
$ yum list
# 查询和keyword相关的包
$ yum search keyword
# 安装
$ yum -y install <packageName>
# 升级
$ yum -y update <packageName>
# 卸载
$ yum -y remove <packageName>

# 软件包组管理命令
# 列出所有可用的软件组列表
$ yum grouplist
# 安装指定软件组
$ yum groupinstall <packageGroupName>
# 卸载指定软件组
yum groupremove <packageGroupName>
```
