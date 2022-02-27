# 使用 Hexo + NexT 打造个性化博客

在本文中，默认 Hexo 博客项目的目录是 `/path/blog`。其中，`path` 代表项目存储路径，`blog` 代表项目名称。

因此当下文中出现如下表述时，代表的含义如表格所示

| 表述                           | 含义                                      |
| ------------------------------ | ----------------------------------------- |
| 站点配置文件                   | `/path/blog/_config.yml`                  |
| 主题配置文件                   | `/path/blog/themes/next/_config.yml`      |
| 在站点中安装                   | 在 `/path/blog/` 下执行命令               |
| 在主题中安装                   | 在 `/path/blog/themes/next/` 下执行命令   |
| 文件 `folder/filename.suffix` | 文件 `/path/blog/folder/filename.suffix` |

## 站点配置

1. 站点基础信息

    ```bash
    title: Epoch's Blog
    subtitle: 我的征途是星辰大海
    description: 人生得意须尽欢，莫使金樽空对月
    keywords: Epoch's Blog,epoch, 梦无涯
    author: 梦无涯
    language: zh-CN
    timezone: UTC
    ```

2. 文章链接样式

    ```bash
    url: https://epoch.fun
    root: /
    permalink: :title.html
    ```

3. 隐藏代码行号

    ```bash
    highlight:
        enable: true
        line_number: false
        auto_detect: false
    ```

4. 更新内容模板
    - 修改“页面模板”文件 `scaffolds/page.md`, 内容如下

        ```bash
        ---
        title: {{ title }}
        type: {{ title }}
        comments: false
        ---
        ```

    - 修改“草稿模板”文件 `scaffolds/draft.md`、“文章模板”文件 `scaffolds/post.md`, 内容如下

        ```bash
        ---
        title: {{ title }}
        permalink: {{ title }}
        description: {{title}}
        keywords: {{title}}
        quicklink: true
        categories:
        tags:
        ---
        ```

## 主题配置

1. 主题外观设置

    ```bash
    # scheme: Muse
    #scheme: Mist
    #scheme: Pisces
    scheme: Gemini
    ```

2. 版权相关设置

    ```bash
    since: 2019         # 显示建站年份
    copyright: epoch    # 显示版权所属，默认使用站点配置文件中的 author

    powered:            # 显示 Powered by Hexo vX.X.X
        enable: false
        version: true
    theme:              # 显示 Powered by NexT - NexT.scheme vX.X.X
        enable: false
        version: true

    beian:              # 显示 ICP 备案信息
        enable: true
        icp: 粤 ICP 备 19020481 号

    creative_commons:       # 显示 “知识共享” 授权协议
        license: by-nc-sa   # 协议类型
        post: true          # 文章底部显示协议图标
    ```

3. 侧边状态栏相关设置
   1. 隐藏侧边状态栏归档、标签、分类信息

        ```bash
        site_state: false
        ```

   2. 社交相关设置

        ```bash
        social:
            GitHub: https://github.com/epochwz || github
            E-Mail: mailto:epoch.wz@gmail.com || envelope

        social_icons:
            enable: true
            icons_only: true
            transition: false
        ```

   3. 显示侧边栏头像

        ```bash
        avatar:
            url: https://bed.epoch.fun/avatar/avatar.jpg
        ```

4. 显示右上角 `Fork on GitHub` 角标

    ```bash
    github_banner:
        enable: true
        permalink: https://github.com/epochwz/blog.git
        title: Fork on GitHub
    ```

5. 显示右下角 “回到顶部” 图标

    ```bash
    back2top:
        enable: true
        sidebar: false      # 显示在侧边状态栏上，默认显示在文章右下角
        scrollpercent: true # 显示当前位置在页面位置的百分比
    ```

6. 启用标签图标

    ```bash
    tag_icon: true
    ```

7. 启用代码块复制按钮

    ```bash
    codeblock:
        border_radius:
        copy_button:
            enable: true
            show_result: true   # 是否显示“复制成功”
            style: mac         # 按钮样式
    ```

8. 网站运行时间统计
    1. 修改文件 `themes\next\_config.yml`

        ```bash
        footer:
            # 统计网站运行时间
            runtime:
                enable: true
                year: 2019
                month: 08
                day: 01
                hour: 00
                minute: 00
                second: 00
        ```

    2. 修改文件 `themes\next\layout\_partials\footer.swig`

        ```bash
        <span class="author" itemprop="copyrightHolder">{{ theme.footer.copyright || author }}</span>
        # 在上面所示代码的下方添加如下代码
        {% if theme.footer.runtime.enable %}
            {% include 'runtime.swig' %}
        {% endif %}
        ```

    3. 添加文件 `themes\next\layout\_partials\runtime.swig`

        ```bash
        <div>
            <span id="sitetime"></span>
            <span id="year" style="display:none">{{theme.footer.runtime.year}}</span>
            <span id="month" style="display:none">{{theme.footer.runtime.month}}</span>
            <span id="day" style="display:none">{{theme.footer.runtime.day}}</span>
            <span id="hour" style="display:none">{{theme.footer.runtime.hour}}</span>
            <span id="minute" style="display:none">{{theme.footer.runtime.minute}}</span>
            <span id="second" style="display:none">{{theme.footer.runtime.second}}</span>
            <script language=javascript>
                function siteTime(){
                    window.setTimeout("siteTime()", 1000);
                    var seconds = 1000;
                    var minutes = seconds * 60;
                    var hours = minutes * 60;
                    var days = hours * 24;
                    var years = days * 365;
                    var today = new Date();
                    var todayYear = today.getFullYear();
                    var todayMonth = today.getMonth()+1;
                    var todayDate = today.getDate();
                    var todayHour = today.getHours();
                    var todayMinute = today.getMinutes();
                    var todaySecond = today.getSeconds();
                    /* Date.UTC() -- 返回 date 对象距世界标准时间 (UTC)1970 年 1 月 1 日午夜之间的毫秒数（时间戳)
                    year - 作为 date 对象的年份，为 4 位年份值
                    month - 0-11 之间的整数，做为 date 对象的月份
                    day - 1-31 之间的整数，做为 date 对象的天数
                    hours - 0（午夜 24 点)-23 之间的整数，做为 date 对象的小时数
                    minutes - 0-59 之间的整数，做为 date 对象的分钟数
                    seconds - 0-59 之间的整数，做为 date 对象的秒数
                    microseconds - 0-999 之间的整数，做为 date 对象的毫秒数 */
                    var year = document.getElementById("year").innerHTML;
                    var month = document.getElementById("month").innerHTML;
                    var day = document.getElementById("day").innerHTML;
                    var hour = document.getElementById("hour").innerHTML;
                    var minute = document.getElementById("minute").innerHTML;
                    var second = document.getElementById("second").innerHTML;// 北京时间 2018-2-13 00:00:00
                    var t1 = Date.UTC(year,month,day,hour,minute,second);
                    var t2 = Date.UTC(todayYear,todayMonth,todayDate,todayHour,todayMinute,todaySecond);
                    var diff = t2-t1;
                    var diffYears = Math.floor(diff/years);
                    var diffDays = Math.floor((diff/days)-diffYears*365);
                    var diffHours = Math.floor((diff-(diffYears*365+diffDays)*days)/hours);
                    var diffMinutes = Math.floor((diff-(diffYears*365+diffDays)*days-diffHours*hours)/minutes);
                    var diffSeconds = Math.floor((diff-(diffYears*365+diffDays)*days-diffHours*hours-diffMinutes*minutes)/seconds);
                    if(diffYears==0){
                    document.getElementById("sitetime").innerHTML=" 本站已运行 "/*+diffYears+" 年 "*/+diffDays+" 天 "+diffHours+" 小时 "+diffMinutes+" 分钟 "+diffSeconds+" 秒";
                    } else{
                    document.getElementById("sitetime").innerHTML=" 本站已运行 "+diffYears+" 年 "+diffDays+" 天 "+diffHours+" 小时 "+diffMinutes+" 分钟 "+diffSeconds+" 秒";
                    }
                }
            siteTime();
            </script>
        </div>
        ```

9. 菜单栏相关设置
    1. 在主题配置文件中启用菜单栏中的菜单项

        ```bash
        menu:
            home: / || home                     # 首页
            about: /about/ || user              # 关于
            tags: /tags/ || tags                # 标签
            categories: /categories/ || th      # 分类
            archives: /archives/ || archive     # 归档

        menu_settings:
            icons: true     # 显示图标
            badges: true    # 显示徽章（标签、分类、归档中的文章数量）
        ```

    2. 新建页面“关于” `source/about/index.md`, 内容如下

        ```bash
        ---
        title: 关于我
        comments: false
        ---
        ```

    3. 新建页面“分类” `source/categories/index.md`, 内容如下

        ```bash
        ---
        title: 文章分类
        type: 'categories'
        comments: false
        ---
        ```

    4. 新建页面“标签” `source/tags/index.md`, 内容如下

        ```bash
        ---
        title: 标签云
        type: 'tags'
        comments: false
        ---
        ```

## 集成第三方服务

### pangu

**插件名称**：[pangu.js](https://github.com/vinta/pangu.js)

**插件作用**：自动在中英文之间添加空格，效果如图

![effect-demo-pangu][]

**安装步骤**

1. 在主题中安装 `theme-next-pangu`

    ```bash
    git submodule add -f https://github.com/theme-next/theme-next-pangu.git source/lib/pangu
    ```

2. 在主题配置文件中启用

    ```bash
    pangu: true
    ```

### reading_progress

**插件名称**：[reading_progress](https://github.com/theme-next/theme-next-reading-progress)

**插件作用**：以进度条的形式在博客页面顶部显示文章阅读进度，效果如图

![effect-demo-reading_progress][]

**安装步骤**

1. 在主题中安装 `theme-next-reading-progress`

    ```bash
    git submodule add -f https://github.com/theme-next/theme-next-reading-progress.git source/lib/reading_progress
    ```

2. 在主题配置文件中启用

    ```bash
    reading_progress:
        enable: true        # 启用 / 禁用
        color: "#37c6c0"    # 进度条颜色
        height: 2px         # 进度条高度
    ```

### lazyload

**插件名称**：[lazyload](https://github.com/tuupola/lazyload)

**插件作用**：实现图片懒加载

**安装步骤**

1. 在主题中安装 `theme-next-lazyload`

    ```bash
    git submodule add -f https://github.com/theme-next/theme-next-lazyload source/lib/lazyload
    ```

2. 在主题配置文件中启用

    ```bash
    lazyload: true
    ```

### quicklink

**插件名称**：[quicklink](https://github.com/GoogleChromeLabs/quicklink)

**插件作用**：预加载当前打开页面可视区域内的链接，实现页面秒开

**安装步骤**

1. 在主题中安装 `theme-next-quicklink`

    ```bash
    git submodule add -f https://github.com/theme-next/theme-next-quicklink.git source/lib/quicklink
    ```

2. 在主题配置文件中启用

    ```bash
    quicklink:
        enable: true
    home: true      # 博客主页默认启用
    archive: true   # 归档页面默认启用
    ```

3. **在需要启用 `quicklink` 的文章中启用**

    ```bash
    ---
    title: 如何在 NexT 中启用 quicklink
    quicklink: true
    ---
    ```

### fancybox

**插件名称**：[fancybox](https://github.com/fancyapps/fancybox)

**插件作用**：提供缩放、预览等更加友好的图片浏览功能，效果如图

![effect-demo-fancybox][]

**安装步骤**

1. 在主题中安装 `theme-next-fancybox3`

    ```bash
    git submodule add -f https://github.com/theme-next/theme-next-fancybox3.git source/lib/fancybox
    ```

2. 在主题配置文件中启用

    ```bash
    fancybox: true
    ```

### 评论系统

**插件名称**：[Gitalk](https://gitalk.github.io)

**插件作用**：使用 `GitHub Repository` 作为博客的评论系统，使用 `GitHub Issue` 存储文章的评论数据

**安装步骤**

1. 注册 [GitHub Application](https://github.com/settings/applications/new)

    ![register-github-application][]

    ![github-application-client-info][]

2. 在主题配置文件中启用

    ```bash
    gitalk:
        enable: true                    # 启用 / 禁用
        github_id: epochwz              # GitHub 用户名
        repo: epoch-wz.github.io        # 用于存储 Issue 的 GitHub 仓库
        client_id: 9d995a899a96999a90c  # GitHub Application Client ID
        client_secret: 59735999997e9559f9b935995c2b699900696955 # GitHub Application Client Secret
        admin_user: epochwz             # 指定 GitHub 仓库所有者、合作者（拥有初始化 Issue 权限的仓库管理者）
    ```

### 访客统计

**插件名称**：[不蒜子统计](http://ibruce.info/)

**插件作用**：统计文章的阅读量、站点的访客量

**安装步骤**：直接在主题配置文件中启用

```bash
busuanzi_count:
    enable: true                # 启用 / 禁用
    total_visitors: true        # 启用 / 禁用 全站访客数量
    total_visitors_icon: user   # 全站访客数量 图标
    total_views: true           # 启用 / 禁用 全站阅读量
    total_views_icon: eye       # 全站阅读量 图标
    post_views: true            # 启用 / 禁用 文章阅读量
    post_views_icon: eye        # 文章阅读量 图标
```

### 全文搜索

**插件名称**：[hexo-generator-searchdb](https://github.com/theme-next/hexo-generator-searchdb)

**插件功能**：实现站点全文搜索功能

**安装步骤**

1. 在站点中安装 NPM 模块 `hexo-generator-searchdb`

    ```bash
    npm --cache-min Infinity install hexo-generator-searchdb --save
    ```

2. 在主题配置文件中启用

    ```bash
    local_search:
        enable: true            # 启用 / 禁用
        # auto 输入时实时显示搜索结果
        # manual 按下回车或者点击搜索按钮时显示搜索结果
        trigger: auto
        top_n_per_article: 1    # 显示每篇文章的前 n 个搜索结果（n=-1 时显示全部）
        unescape: false         # 反转义 HTML 字符串
    ```

3. 可选步骤：在 Hexo 站点配置文件中添加个性化配置

    ```bash
    search:
        path: search.xml    # 生成的文件路径，默认 XML 格式，若此处以 .json 结尾则输出文件是 Json 格式
        # 搜索范围
        # post  在文章中搜索，默认项
        # page  在页面中搜索
        # all   在文章和页面中搜索
        field: post
        # 搜索结果显示格式
        # html      原始的 HTML 格式
        # raw       原始的 Markdown 格式
        # excerpt   只显示摘要信息
        format: html
        limit: 10000        # 显示的最大搜索结果数量
    ```

### 相关文章

**插件名称**：[hexo-related-popular-posts](https://github.com/tea3/hexo-related-popular-posts)

**插件功能**：显示每篇文章的相关文章（根据相同标签的数量计算相关度）

**安装步骤**

1. 在站点中安装 NPM 模块 `hexo-related-popular-posts`

   ```bash
   npm --cache-min Infinity install hexo-related-popular-posts --save
   ```

2. 在主题配置文件中启用插件

    ```bash
    related_posts:
        enable: true    # 启用 / 禁用
        title:          # 自定义标题，默认是“相关文章”
        display_in_home: false  # 主页是否显示
        params:
            maxCount: 8         # 最大显示数量
            #PPMixingRate: 0.0
            #isDate: false
            #isImage: false
            #isExcerpt: false
    ```

### 字数统计

**插件名称**：[hexo-symbols-count-time](https://github.com/theme-next/hexo-symbols-count-time)

**插件作用**：统计文章 / 站点的字数、估计文章的阅读时间

**安装步骤**

1. 在站点中安装 NPM 模块 `hexo-symbols-count-time`

    ```bash
    npm --cache-min Infinity install hexo-symbols-count-time --save
    ```

2. 在站点配置文件中添加模块设置

    ```bash
    symbols_count_time:             # 字数统计配置节点
        symbols: true               # 启用 / 禁用 文章字数统计
        time: true                  # 启用 / 禁用 文章阅读时间预计
        total_symbols: true         # 启用 / 禁用 全站字数统计
        total_time: false           # 启用 / 禁用 全站阅读时间预计
        exclude_codeblock: false    # 排除 / 统计 代码块中的字符
    ```

3. 在主题配置文件中进行个性化配置

    ```bash
    symbols_count_time:
        separated_meta: false   # 是否单独一行显示
        item_text_post: false   # 是否显示文章统计的文字说明
        item_text_total: false  # 是否显示全站统计的文字说明
        # 单词平均字符长度
        # 中文 2
        # 英文 5
        awl: 5
        # 平均每分钟阅读的单词数量
        # 慢速 200
        # 正常 275
        # 快速 350
        wpm: 200
    ```

### 优化界面样式

1. 优化文章归档界面
   1. 将 `themes\next\languages\zh-CN.yml` 中的“日志” 替换成 “文章”
   2. 禁止显示 cheers 相关信息：编辑 `themes\next\layout\archive.swig`

        ```bash
        {%- if theme.cheers %}
            <span class="archive-move-on"></span>
            <span class="archive-page-counter">
                {%- set cheers %}
                {%- set posts_length = site.posts.length %}
                {%- if posts_length > 210 %} {%- set cheers = 'excellent' %}
                {% elif posts_length > 130 %} {%- set cheers = 'great' %}
                {% elif posts_length > 80 %} {%- set cheers = 'good' %}
                {% elif posts_length > 50 %} {%- set cheers = 'nice' %}
                {% elif posts_length > 30 %} {%- set cheers = 'ok' %}
                {% else %}
                {%- set cheers = 'um' %}
                {%- endif %}
                {{ __('cheers.' + cheers) }}! {{ _p("counter.archive_posts", site.posts.length) }} {{ __('keep_on') }}
            </span>
        {%- endif %}
        # 将以上内容修改成以下内容
        {%- if theme.cheers %}
            <span class="archive-move-on"></span>
            <span class="archive-page-counter">
                {{ _p("counter.archive_posts", site.posts.length) }}
            </span>
        {%- endif %}
        ```

2. 优化 footer 样式

## SEO

1. 在站点中添加蜘蛛协议 `source/robots.txt`

    ```bash
    User-agent: *
    Allow: /
    Allow: /archives/
    Allow: /categories/
    Allow: /tags/
    Allow: /about/

    Sitemap: https://epoch.fun/sitemap.xml
    Sitemap: https://epoch.fun/baidusitemap.xml
    ```

    `Allow` 指定允许搜索引擎爬取的路径，可以根据 **主题配置** 文件中的 `menu` 配置的菜单来指定

    `Disallow` 指定禁止搜索引擎爬取的路径

    `Sitemap` 指定站点地图文件的 URL, 博客发布后应该能够在浏览器中通过此 URL 访问到该文件

2. 在站点中安装 NPM 模块来生成站点配置文件

    ```bash
    # 用于生成通用的站点地图文件
    npm --cache-min Infinity install hexo-generator-sitemap --save
    # 用于生成百度专用的站点地图文件
    npm --cache-min Infinity install hexo-generator-baidu-sitemap --save
    ```

3. 在站点配置文件中添加站点地图配置

    ```bash
    # SEO
    sitemap:                    # 指定网站通用站点地图文件路径
        path: sitemap.xml
    baidusitemap:               # 指定百度专用站点地图文件路径
        path: baidusitemap.xml
    ```

4. 完成以上步骤后，每次执行 `hexo g` 时都会自动在 `public/` 目录下生成网站的蜘蛛协议文件 `robots.txt` 和站点地图文件 `sitemap.xml` `baidusitemap.xml`
5. 提交 `sitemap.xml` 到 [Google Search Console](https://search.google.com/search-console/welcome)
6. 记录下 `Google Search Console` 提供的 `google_site_verification` 的值（类似 `OmNpqiry-mHFjAWMqnhxF3tBIFFF-s56G_iDGvdInYD` 的字符串）, 后续会用到
7. 在主题配置文件中启用 SEO 优化

    ```bash
    disable_baidu_transformation: true
    canonical: true
    seo: true
    index_with_subtitle: true
    exturl: true # 加密外链，相当于 "nofollow"
    google_site_verification: OmNpqiry-mHFjAWMqnhxF3tBIFFF-s56G_iDGvdInYD
    baidu_push: true
    ```

[register-github-application]:../images/hexo-next/register-github-application.png
[github-application-client-info]:../images/hexo-next/github-application-client-info.png
[effect-demo-reading_progress]:../images/hexo-next/effect-demo-reading_progress.png
[effect-demo-pangu]:../images/hexo-next/effect-demo-pangu.png
[effect-demo-fancybox]:../images/hexo-next/effect-demo-fancybox.png