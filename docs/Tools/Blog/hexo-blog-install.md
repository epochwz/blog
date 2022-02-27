# 使用 Hexo + NexT 搭建个人博客

## 简介

> [Hexo][] 是一个快速、简洁且高效的博客框架。
>
> [NexT][] 是一个优雅的 Hexo 主题

Hexo 相当于一个 Markdown 渲染引擎，它可以通过使用 NexT 等主题预定义好的样式，将 Markdown 文件渲染成 HTML 文件，然后只需要将其生成的静态文件托管到一个 Web 服务器上，就可以完成一个静态博客的部署和访问了。

## 安装

1. 安装 [Git](https://git-scm.com/)
2. 安装 [Node.js](https://nodejs.org/zh-cn/)（版本要求在 v6.9.0 以上）
3. 安装 Hexo

    ```bash
    # 使用淘宝镜像，加快安装速度
    npm config set registry https://registry.npm.taobao.org
    # 安装 Hexo 命令行工具
    npm --cache-min Infinity install -g hexo-cli
    ```

4. 初始化 Hexo 博客
   1. 克隆 `hexo-starter` 项目

        ```bash
        # path 是博客项目的存储路径，blog 是博客项目的名称，两者均可自定义
        git clone https://github.com/hexojs/hexo-starter /path/blog
        ```

   2. 删除 `.gitmodules` 、 `.git/` 、 `themes/landscape/`
   3. 可选步骤：替换 `.gitignore` 内容如下

        ```bash
        /*

        !.gitignore
        !.gitmodules

        !README.md
        !LICENSE

        !scaffolds
        !source/
        !themes/
        !_config.yml
        !package.json
        ```

   4. 重新初始化成 Git 项目

        ```bash
        # 进入博客项目目录，path 替换成实际路径
        cd /path/blog
        # 初始化并提交
        git init
        git add .
        git commit -m "初始化 Hexo 博客"
        ```

5. 安装 NexT 主题
    1. 在站点配置文件 `/path/blog/_config.yml` 中启用 `NexT` 主题

        ```bash
        theme: landscape
        # 修改成
        theme: next
        ```

    2. 在 GitHub 上 **fork** 主题仓库 [hexo-theme-next][]
    3. 以 [Git Submodule][] 的方式添加 Next 主题（方便以后继续获取主题更新）

        ```bash
        # 添加
        git submodule add git@github.com:epochwz/hexo-theme-next.git themes/next
        # 提交
        git add .
        git commit -m "初始化 Hexo 博客主题：以 Git Submodule 的方式添加 NexT 主题项目"
        ```

6. 安装 NPM 模块

    ```bash
    npm --cache-min Infinity install
    ```

7. 启动博客

    ```bash
    hexo server # 启动一个本地服务器，可以通过 http://localhost:4000 预览博客
    ```

8. 访问博客：直接在浏览器中打开 <http://localhost:4000> 即可

至此，一个 Hexo 博客的基础骨架就搭建好了，如果没有个性化需求的话，就可以开始愉快地写文章了

- 使用 [Markdown][] 格式在 `source/_posts/` 目录下写文章
- 使用 `hexo server` 在本地预览博客效果
  - 除非该命令启动的本地服务器被关闭了，否则只需要启动一次就可以了

## 使用

此处只列举一些常用的命令，更详细的 Hexo 使用（写作）教程可以参考 [Hexo Docs][]

```bash
# 在当前项目下初始化一个 Hexo 博客，相当于克隆 hexo-starter 和 hexo-theme-landspace 项目
hexo init blog

# 写作：使用 Hexo 命令，可以根据 scaffolds 中的模板文件，方便的新建 文章、草稿、页面
hexo new title          # 新建一篇文章 _posts/title.md
hexo new draft title    # 新建一篇草稿 _drafts/title.md
hexo new page title     # 新建一个页面 title/index.md

hexo g      # 生成
hexo d      # 部署
hexo g -d   # 生成并部署
```

## 部署

### 使用 GitHub Pages 部署

1. 在 `GitHub` 上新建 `Repository`, 名称必须是 `{github-username}.github.io`

   ![new-repository-for-blog][]
2. 安装 NPM 模块 `hexo-deployer-git`

    ```bash
    npm --cache-min Infinity install hexo-deployer-git --save
    ```

3. 在站点配置文件中配置远程仓库地址

    ```bash
    deploy:
        type: git       # 推送方式
        repo: git@github.com:epochwz/epochwz.github.io.git    # 仓库地址
    ```

4. 生成博客静态文件并部署（推送到 `Git Repository` 远程分支）

    ```bash
    hexo g -d
    ```

5. 访问 <https://epochwz.github.io>

### 使用 VPS 部署

1. 搭建 Git 服务器
    1. 新建 Linux 用户 `git`

        ```bash
        sudo useradd git -m -d /home/git -s /usr/bin/git-shell
        ```

    2. 新建 Git 裸仓库（只包含版本控制信息，不包含项目文件）

        ```bash
        git init --bare /home/git/epoch/blog.git
        ```

        `epoch` 相当于 GitHub 中的用户名，可以自定义

        `blog.git` 相当于 GitHub 中的仓库名，可以自定义

    3. 可选测试：在本地克隆仓库

        ```bash
        git clone git@ip:epoch/blog.git
        ```

2. 安装 [Nginx](https://www.nginx.com/)
3. 配置代理

    ```bash
    server{
        listen 80;
        server_name epoch.fun;

        location / {
            root /home/git/epoch/blog;
            index index.html index.htm;
        }
    }
    ```

4. 安装 NPM 模块 `hexo-deployer-git`

    ```bash
    npm --cache-min Infinity install hexo-deployer-git --save
    ```

5. 在站点配置文件中配置远程仓库

    ```bash
    deploy:
        type: git
        branch: master # Git 分支，如果是 master 可以不配置
        repo: git@epoch.fun:epoch/blog.git
    ```

6. 生成博客静态文件并部署（推送到 `Git Repository` 远程分支）

    ```bash
    hexo g -d
    ```

7. 访问 <https://epoch.fun>

### 同时部署到多个 Git 服务器上

在站点配置文件中配置多个 Git 远程仓库

```bash
deploy:
  - type: git
    repo: git@github.com:epochwz/epochwz.github.io.git
  - type: git
    repo: git@epoch.fun:epoch/blog.git
```

## 更新 NexT 主题

1. 首次更新

    ```bash
    # 追踪官方仓库
    git remote add upstream https://github.com/theme-next/hexo-theme-next.git
    # 继续步骤 2
    ```

2. 后续更新

    ```bash
    # 拉取官方远程仓库的更新
    git fetch upstream/master
    # 合并更新（并解决冲突）
    git merge upstream/master
    # 推送更新
    git push
    ```

## 在其他电脑克隆项目

```bash
git clone --recursive git@github.com:epochwz/blog.git
cd blog/themes/next
git checkout master
```

[Hexo]:https://hexo.io/zh-cn/
[NexT]:https://theme-next.org/
[hexo-theme-next]:https://github.com/theme-next/hexo-theme-next
[Git Submodule]:https://git-scm.com/book/zh/Git-%E5%B7%A5%E5%85%B7-%E5%AD%90%E6%A8%A1%E5%9D%97
[Markdown]:https://guides.github.com/features/mastering-markdown/
[Hexo Docs]:https://hexo.io/zh-cn/docs/writing

[new-repository-for-blog]:/docs/images/hexo-next/new-repository-for-blog.png