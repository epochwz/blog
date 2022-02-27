---
date: 2017-12-07T15:36:35.000Z
title: Git(伍)|场景进阶篇
tags:
  - Tools
  - Git
categories:
  - Tools
  - Git
---

**版本回退**

```bash
# 当前Commit=A,A的前一个Commit=B


# 将HEAD从A指向B，不改变Work和Stage状态
git reset HEAD^1 --soft

# 将HEAD从A指向B，重置Stage,不改变Work状态
git reset HEAD^1

# 将HEAD从A指向B，重置Stage和Work状态
git reset HEAD^1 --hard
```

**查看区别**

```bash
# Work和Stage区别
git diff

# Work和HEAD区别
git diff HEAD

# Stage和HEAD区别
git diff --stage
```

**储藏现场**

```bash
# 储藏未提交的工作目录状态
git stash

# 恢复储藏的工作目录状态
git stash apply
# 删除储藏
git stash drop

# 恢复并删除储藏的工作目录状态
git stash pop

# 查看所有储藏
git stash list
```

**获取服务器上最新版本并将本地主分支指向该版本**

```bash
git reset --hard origin/master
```

**丢弃所有本地改动与提交**

```bash
git fetch origin
```

**检查 Git 的某一项配置**

```bash
git config <key>
```

#### 忽略指定文件

[gitignore List](https://github.com/github/gitignore)

* 忽略系统自动生成的文件
* 忽略编译生成的中间文件、可执行文件
* 忽略敏感信息配置文件

```bash
# 强制添加被忽略的文件
git add -f fileName
# 检查忽略规则
git check-ignore -v fileName
```

#### 配置文件

当前仓库配置文件.git/config
当前用户配置文件.gitconfig

将修改并入上一次的提交

```bash
git add .
# 修改上一次提交的说明信息
git commit --amend
```

**提交到了错误的分支**

```bash
git reset HEAD~ --soft
git stash
git checkout targetBranch
git stash pop
git add .
git commit -m "message"
```

#### **编译安装**

```bash
# 安装依赖
sudo apt-get install curl-devel expat-devel gettext-devel openssl-devel zlib-devel

# 下载Git源码包(<https://github.com/git/git/releases>)
wget https://github.com/git/git/archive/v2.13.12.tar.gz

# 解压
tar -zxvf v2.13.12.tar.gz
# 进入解压目录
cd git-2.13.12

make prefix=/usr/local all
make prefix=/usr/local install

vim /etc/ld.so.conf
   /usr/local/lib

git --version
```

#### 原理：基于 SSH 协议的 Git

* Git 命令实质上是通过 SSH 登录服务器，并执行相关命令(能够执行的命令受 git-shell 限制)
* **Example:** `git push` 相当于 `ssh git@github.com "git-receive-pack /home/git/username/repoName.git"`
* **Git URL** `git clone git@github.com:username/repoName.git`

  * 用户名`git`
  * 服务器地址`github.com`，默认端口22
  * 项目目录`username/repoName.git`

* 可以尝试直接登录 GitHub 服务器`ssh git@github.com`(事实上确实可以登录成功，但是命令被篡改，输出一段提示信息后就把你的连接关了)