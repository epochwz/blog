# Visual Studio Code

## 零 | 为什么选择 VSCode

Visual Studio Code ( 简称 VS Code ) 是由微软开发的轻量级文本编辑器，具备开源、跨平台、高颜值的特点。

- **写代码**: VSC 支持几乎所有主流开发语言的语法高亮、代码补全，且内置 Git、Diff、Debug、JavaScript Runtime 等杀手级功能，同时还拥有大量强大的插件
- **写文档**: VSC 内置了 Markdown 预览功能，开箱即用，无需折腾

相比于 SublimeText3, VS Code 内置了 Emmet、Markdown Preview、代码补全等强大功能，无需折腾，开箱即用

相比于 Atom, 虽然同样是基于 Web 技术开发，VS Code 在性能方面远远胜过 Atom

长远来看，SublimeText3 闭源收费（目前可免费使用，不排除某天完全付费)，而 VS Code、Atom 则开源免费。在开源社区的支持下，后二者发展势必更加迅猛。

因此，我最终选择了 VS Code 作为我的主力文本编辑器，并在此记录自己的常用配置，以作备忘

## 壹 | 同步编辑器配置

从接触一款新的编辑器开始到使用的得心应手，需要付出一定的精力去琢磨自己的偏好设置，包括快捷键、插件、主题和其他设置。因此，实现 `一次配置，到处使用`是十分必要的。

使用插件 [Settings Sync][] + [GitHub Gist][] 可以轻松的同步 VS Code 的配置，以便在任何时间、任何电脑上快速的恢复自己熟悉的工作环境

具体操作步骤如下：

1. 安装插件 `Settings Sync`

    ![Extension-Install][]

2. 上传配置： 使用快捷键 `Shift+Alt+U`，会自动在浏览器打开 GitHub Token 的管理界面

    ![Token-Manage][]

    如果没有自动打开，可以自行登录 GitHub, 依次点击 `Settings -> Developer settings -> Personal access tokens` 进入该页面
3. 生成新的 Token：`点击 Generate New token -> 填写该 Token 的描述信息 （如下图所示） -> 勾选 gist -> 点击 Generate token`

    ![Token-Generate][]

4. 复制生成的 Token (GitHub 仅显示一次，需妥善保管，防止丢失)

    ![Token-Save][]

5. 在 **步骤 2** 的弹出框中输入生成的 Token, 回车便可完成配置上传到 Gist.

    **妥善保管生成的 Gist ID, 以后在其他电脑下载配置时需要用到**

    ![Sync-upload][]

至此，你已经将编辑器的配置上传到 `Github Gist` 上了，你可以在本机进行配置的上传 / 下载
![Sync-Gist][]

- 上传配置 `Shift+Alt+U(Sync: Update / Upload Settings)`
- 下载配置 `Shift+Alt+D(Sync: Download Settings)`

在新电脑上同步 （下载） 配置：`Shift+Alt+D`, 在弹出框中依次输入之前生成的 `GitHub Token` 和 `Gist ID` 即可

## 插件

| 插件名称                                                  | 功能                         | 备注 |
| --------------------------------------------------------- | ---------------------------- | ---- |
| Chinese (Simplified) Language Pack for Visual Studio Code | 中文支持                     |      |
| Settings Sync                                             | 同步编辑器配置               |      |
| Prettier                                                  | JavaScript 格式化            |      |
| markdownlint                                              | Markdown 标准格式检查        |      |
| Markdown Preview Github Styling                           | Markdown GitHub 样式预览效果 |      |
| Markdown All in One                                       | Markdown 快捷键              |      |
| Text Tables                                               | 表格编辑和格式化             |      |
| writing4cn                                                | 在中英文之间插入空格         |      |

## 快捷键

| 快捷键           | 功能               | 来源               |
| ---------------- | ------------------ | ------------------ |
| **File**         |                    |
| Ctrl+N           | 新建无标题文件     | Default            |
| Ctrl+Alt+N       | 侧边栏新建文件     | 自定义             |
| Ctrl+Shift+Alt+N | 侧边栏新建文件夹   | 自定义             |
| **Edit**         |                    |
| Alt+↑/↓          | 向上/下移动行      |                    |
| Shift/Ctrl+Enter | 上/下方新建一行    | 自定义             |
| **Markdown**     |                    |
| Ctrl+T           | 创建 Markdown 表格 | Text Tables |
| Ctrl+B           | 粗体               |                    |
| Ctrl+I           | 斜体               |                    |

| 操作       | Command                        | 快捷键       | 来源    | 类型 |
| ---------- | ------------------------------ | ------------ | ------- | ---- |
| 格式化代码 | `editor.action.formatDocument` | Ctrl+Alt+L   | Default | 修改 |
| 合并行     | `editor.action.joinLines`      | Ctrl+Shift+J | Default |

## 其他设置

- 关闭代码缩略图 `"editor.minimap.enabled": false`

## 彻底卸载

在控制面板中卸载完成后，删除目录 `%APPDATA%\Code` 和 `%HOMEPATH%\.vscode`

[Settings Sync]:https://marketplace.visualstudio.com/items?itemName=Shan.code-settings-sync

[GitHub Gist]:https://gist.github.com/

[Extension-Install]:../images/vscode/Extension-Install.png

[Token-Manage]:../images/vscode/Token-Manage.png

[Token-Generate]:../images/vscode/Token-Generate.png

[Token-Save]:../images/vscode/Token-Save.png

[Sync-upload]:../images/vscode/Sync-upload.png

[Sync-Gist]:../images/vscode/Sync-Gist.png