# IDEA Personalized

> ! **Version**: IntelliJ IDEA 2019.2.4 (Ultimate Edition)

1. 安装、自定义插件配置、启动
2. [License Activation](#license-activation)
3. [配置云同步](#配置云同步)
4. [个性化配置](#个性化配置)
5. [插件配置](#插件配置)

## License Activation

1. 选择免费试用：`Evalutate for Free -> Evaluate`
2. 下载破解文件 [jetbrains-agent.jar]
3. 修改启动参数 `Configure -> Edit Custom VM Options`

    ```bash
    # 在打开的文件末尾追加以下内容：参数是破解文件在电脑上的存储路径，可以根据需要自行修改
    -javaagent:C:\Users\epoch\.IntelliJIdea2019.2\config\settingsRepository\repository\jetbrains-agent.jar
    ```
    <!-- TODO 更换破解文件的存储位置和下载链接 -->

4. 保存后重启 IDEA
5. 打开激活页面：`Configure -> Manage License`
6. 如果有旧的 `LICENSE` 则必须移除：`Remove license`，没有则直接下一步
7. 选择 `License Server` 方式激活：`http://jetbrains-license-server`

## 配置云同步

1. 创建 [GitHub Repository][]: `idea-settings`, 用于保存 IDEA 的配置文件
2. 创建 [GitHub API Token][], 用于授权 IDEA 访问 GitHub

   ![Generate GitHub API Token for Repo][]

3. 同步配置：`Configure -> Settings Repository -> 填写远程仓库链接 -> Overwrite remote`

   ```text
   https://github.com/epochwz/idea-settings.git
   ```

4. （可选）关闭自动同步功能：`Configure -> Settings -> Tools -> Settings Repository -> Auto Sync -> 取消 -> OK`

## 个性化配置

### 界面配置

- 显示工具按钮 `View -> ToolButtons`
- 主题和字体 `Default Settings -> Appearance`

## 插件配置

### Markdown Navigator

`File | Settings | Languages & Frameworks | Markdown`

- **Application Settings**
  -  关闭目录链接：`Allow directories as link targets`
- **Editor**
  - Editor Settings
    - 编辑器布局切换：`Toggle Editor Layout -> Text/Preview Layout`
    - 设置链接根路径：`Default Link completion format -> Repo Relative`
    - 开启自动软换行：`Soft Wrap -> Enabled`
    - 关闭面包屑导航： `Show Breadcrumbs`
  - 智能编辑全部开启：`Smart Keys` & `Language Injections` & `Intentions & Annotations`
    - `Punctuation Chars`: 添加中文符号（切换文本样式时排除单词末尾的标点符号）
- **Preview**

    ![Markdown-Navigator-Settings-Preview]

- **Stylesheet**
  - `Scheme -> Default`
  - `Stylesheet ->  Default JavaFx Stylesheet`
- 关闭异常反馈：`Exceptions Reports -> Track Exception Reports`

[GitHub Repository]:https://github.com/new
[GitHub API Token]:https://github.com/settings/tokens/new
[jetbrains-agent.jar]: https://zhile.io/2018/08/25/jetbrains-license-server-crack.html

[Generate GitHub API Token for Repo]:/docs/images/tools/generate-github-api-token-for-repo.png
[Markdown-Navigator-Settings-Preview]: /docs/images/Markdown-Navigator-Settings-Preview.png
