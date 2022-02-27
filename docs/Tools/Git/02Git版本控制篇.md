---
date: 2017-12-04T15:36:35.000Z
title: Git(贰)|版本控制篇
tags:
  - Tools
  - Git
categories:
  - Tools
  - Git
---

## Git 仓库(Repository)

* **工作目录(Working Directory)：**当前工作目录(HEAD+刚做出的修改)

* **版本库(History)：**保存项目文件的历史版本

  * **缓存区(Stage/Index)：**保存工作目录的修改(工作目录某一时刻的快照，准备用于提交成正式版本)

  * **当前版本(HEAD)：**项目的一个最新版本

**简单安装**

```
# Ubuntu
sudo apt-get install git
# CentOS
```

Windows 用户直接上官网下载，打开`git bash` 使用,强烈建议使用 Linux 系统，简单易上手

**基本配置**

```
# 配置Git账号信息(当你使用 Git 提交时，Git 需要记录提交的人是谁)
git config --global user.name "username"
git config --global user.email "email@example.com

# 验证
git --version

# Windows配置
# 忽略Windows/Unix换行符的转换
git config --global core.autocrlf false
# 避免git status显示的中文文件名乱码
git config --global core.quotepath off
# 设置大小写敏感
git config --global core.ignorecase false
```

## 本地仓库\(Local Repository\)

**创建项目 Repository**

```
# - 创建并初始化新的 Repository “gitTest"
git init gitTest
# - 初始化已存在的项目
cd gitTest && git init

Initialized empty Git repository in /home/s3kj-jwz/gitTest/.git/
初始化一个空的 Git Repository,即.git为项目gitTest的版本库
```

**查看 Repository 状态**

```
git status


On branch master
在 master 分支上(分支概念以后会讲，可以暂不理会)
Initial commit
初始化提交
nothing to commit (create/copy files and use "git add" to track)
没有东西可以提交(通过 git add 跟踪文件)
```

### **版本控制**

#### v1.0

编写一个 README.md 文件

```
touch README.md
git status

Untracked files:
  (use "git add <file>..." to include in what will be committed)

    README.md
未跟踪文件(使用git add 跟踪该文件 README.md)
nothing added to commit but untracked files present (use "git add" to track)
没有可以提交的但是当前有未跟踪文件
```

将README.md 添加到版本控制中去,即让git管理该文件的修改

```
git add README.md

有改动需要提交
Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
  使用 git rm --cached 撤销跟踪该文件
    new file:   README.md
```

将等待提交的README.md提交到版本库中

```
git commit -m "Initial commit:add README.md"


[master (root-commit) c030b25] Initial commit:add README.md
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 README.md
 当前版本增加了一个README.md
```

现在，你已经完成了项目的第一个版本，并且把他保存到了版本库中，无论你今后做什么修改，你都可以瞬间将一切还原到这个版本.

看一下工作目录的状态

```
git status

nothing to commit, working directory clean
没有东西需要提交，工作目录是干净的(工作目录与版本库的最新版本HEAD一致)
```

#### v2.0

v1.0增加了README.md

现在我们开始v2.0的工作，当然我们还要做更多的事情，修改README.md

```
v1.0完成了，现在开始v2.0
```

查看工作目录状态

```
Changes not staged for commit
修改没有暂存以便提交
意思是你修改了 README.md，应该将它添加到暂存区，好提交到一个新的版本
```

重复v1.0 时的步骤，添加到暂存区并提交，完成v2.0的工作

现在是深夜了，你有点神志不清，于是你打开了README.md，胡乱写了一些东西

第二天醒来，你习惯性的查看一下仓库状态，发现git提示你README.md被修改了

于是你想查看一下哪里被修改了

```
git diff

diff --git a/README.md b/README.md
index 4747543..5d5c92d 100644
--- a/README.md
+++ b/README.md
@@ -1 +1,2 @@
 v1.0完成了，现在开始v2.0
+我现在有点神志不清，这是我乱写的东西
```

哦，原来是昨晚发疯乱写了一句。撤销工作区相对于暂存区的修改

```
git checkout -- README.md
```

版本回调

```
git reset HEAD README.md
```

删除文件

```
rm README.md
```

添加删除操作到暂存区

```
git rm README.md
```

提交

#### Tip

版本控制只能跟踪文本文件的变动

## 远程仓库\(Remote Repository\)

**从本地仓库关联远程仓库**

```
# SSH认证
# 关联远程仓库
git remote add <remote-repo-name> server_user@server:/path/repoName.git
# Example
git remote add origin git@github.com:s3kj-mwy/shell.git


# api-token认证
git remote add <remote-repo-name> https://username:apiToken@domain/path/repoName.git

# 首次推送本地仓库到远程仓库
git push -u origin master
# -u 关联远程分支
# master 指定远程分支
```

**从远程仓库克隆到本地仓库**

```
git clone /path/to/repository
# 克隆下来的自动关联好远程仓库了
git@github.com:username/projectName.git
```

**操作远程仓库**

```
# 推送
git push origin master

# 查看关联的远程仓库信息
git remote -v

# 删除关联的远程仓库
git remote rm origin <remote-repo-name>
```

**检出远程分支**

```
git checkout -t origin/branchName
git checkout -b branchName origin/branchName
```

**创建和远程分支关联的本地分支**

```
git checkout -b branch-name origin/branch-name
```

**拉取最新提交**

```
git pull
```

**关联远程分支**

```
git branch --set-upstream branch-name origin/branch-name
```



