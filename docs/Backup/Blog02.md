---
author: s3kj-mwy
email: s3kj.mwy@gmail.com
blog: http://blog.s3kj.xin
GitHub: https://github.com/s3kj-mwy

date: 2017-12-02 01:23:13
title: 进击的博客(贰) | Hexo 框架
tags: [Blog,Hexo]
categories: Blog
---
> 本文状态：持续、频繁更新
> 

__Hexo 是什么？__
> Hexo 是一个使用 Node.js 实现的静态博客框架，用于将 Markdown 文件解析成 HTML 文件，加上 自定义的主题文件组装成一个静态博客网站

__Hexo 静态博客 的构成__
> Hexo 静态博客=Hexo 核心框架+ Hexo 主题+ Markdown 文件
> 
> Markdown: 博客文章(普通用户唯一需要关心的东西)
> Hexo 主题：美化生成的 HTML 页面(框架自带，也可以在GitHub找顺眼的，还可以自定义)
> Hexo 核心框架：组装 文章+主题(一般只需要改改配置文件就行了)

### 安装 Hexo 框架
__安装Node.js__
```
# Install nvm
wget -qO- https://raw.githubusercontent.com/creationix/nvm/v0.33.6/install.sh | bash
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"

# Install Node.js
nvm install stable

# 解决 npm 全局安装的权限问题
npm config set unsafe-perm true
```
__安装 Hexo__
```
# Install Hexo
npm install -g hexo-cli
```

__安装验证__
```
hexo version
```

### Hexo 生成 静态博客网站

__新建一个 Hexo 博客网站__
```
hexo init <blogName>
cd <blogName> && npm install
```

__网站的目录结构__
```
.
├── _config.yml     # Hexo 配置文件
├── package.json    # Node.js 依赖的模块
├── scaffolds       # 博客文章的模板文件
├── source          # 博客的资源文件夹
|   ├── _drafts     # 草稿文章
|   └── _posts      # 博客文章
└── themes          # 博客的主题文件
```

__写文章(Markdown)__
```
# 使用 Hexo 命令生成文章模板
hexo new "title"

# 只要将 Markdown 文件放在 _posts 文件夹下即可
```

__生成文章(HTML)__
```
# Hexo 将 _posts 文件夹下的 Markdown 和 HTML 文件解析，组合主题文件，生成 HTML 页面到 public 文件夹，其他格式文件直接拷贝过去
hexo generate
```

### Hexo 本地部署
```
# 实时监听文件变动，生成博客网站 通过 http://localhost:4000 访问
hexo server
```

### Hexo 远程部署(GitHub Pages)
__GitHub 上新建 Repository:__项目名称必须是`username.github.io`

__Hexo 远程部署配置__
```
# 修改配置文件config.yml
deploy:
    type: git
    repo: git@github.com:username/username.github.io.git
    branch: master
```

__安装远程部署插件__
```
npm install hexo-deployer-git --save
```

__发布网站__`hexo deploy`

## Hexo 远程部署(VPS) 
### 搭建 Git 服务器
__添加 Git 用户__
```
# 添加用户(指定主目录、登录shell)
sudo useradd git -m -d /home/git -s /usr/bin/git-shell
```

__创建 Git 仓库__
```
# 创建一个 裸仓库：只有版本库(HEAD)，没有工作区和暂存区
# 服务器上的 版本库 仅仅为了项目共享，原则上禁止直接操作该目录
git init --bare /home/git/s3kj-mwy/Blog.git
```

__赋予 Git 用户权限__
```
chown -R git:git /home/git
```

__客户端使用__
```
# 添加公钥到服务器 git 用户
ssh-copy-id -i ~/.ssh/id_rsa.pub git@s3kj.xin

# 克隆 Blog Repository
git clone git@s3kj.xin:s3kj-mwy/Blog.git
```

### Git Hook 自动部署

__Git Hook 是什么?__
> Git Hook(钩子)相当于一个触发器，当 Repository 收到 push/commit 等 git 操作后，会执行一个特定的脚本文件完成相应的功能。

[Git 支持的 Git Hook 介绍](https://git-scm.com/book/zh/v2/%E8%87%AA%E5%AE%9A%E4%B9%89-Git-Git-%E9%92%A9%E5%AD%90)

__怎么实现博客自动部署？__
> 我们要用到的钩子就是 `post-update` ，当 服务器上的 <u>Blog Repository 版本库</u> 收到 `git push` 后触发脚本，使服务器上的<u>博客网站目录</u> 自动拉取更新

__post-update 脚本内容__
```
git clone -b Blog
```

#### 禁止 git 用户执行系统 shell
1. 设置用户登录 shell ： git-shell，相当于一个沙盒环境，只允许执行沙盒内包含的命令     
    + 默认命令`git-receive-pack <argument>`
    + 默认命令`git-upload-pack <argument>`
    + 默认命令`git-upload-archive  <argument>`
    + __自定义命令白名单：__创建目录`/home/git/git-shell-commands/`，存放想执行的命令
2. `/home/git/.ssh/authorized_keys`中每行 ssh-key 前使用 command 命令覆盖原本的命令
    + command="echo 'You are not allow to login!'" ssh-key
    + command="git-shell -c \"$SSH_ORIGINAL_COMMAND\"" ssh-key