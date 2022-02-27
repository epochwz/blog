# Git - Configuration

## 基本概念

**配置文件**

- 仓库级配置    `.git/config`
- 用户级配置    `~/.gitconfig`
- 系统级配置
  - Windows:    `C:\Program Files\Git\mingw64\etc\gitconfig`
  - Linux:      `/etc/gitconfig`

> ! 配置文件（冲突时）的优先级顺序：仓库级 > 用户级 > 系统级

## 基本操作

**查看配置**

```bash
# 查看全部配置
git config -l [--local|--global|--system]

git config -l --system   # 查看系统级配置
git config -l --global   # 查看用户级配置
git config -l --local    # 查看仓库级配置
git config -l            # 查看已生效配置

# 查看单个配置
git config --get section.key [--local|--global|--system]
```

**编辑配置**

```bash
# 编辑配置文件，默认 --local
git config -e [--local|--global|--system]

git config -e --system    # 编辑系统级配置
git config -e --global    # 编辑用户级配置
git config -e [--local]   # 编辑仓库级配置

# 设置单个配置，默认 --local
git config section.key value [--local|--global|--system]
```

## 常用配置

### Common

**避免中文路径乱码**

```bash
# 控制是否在输出文件路径时对特殊字符进行转义显示
#   true:   如果文件路径中的字符大于 0x80 时，则进行转义显示
#   false:  不对文件路径进行转义显示
git config --global core.quotepath false
```

### Windows

**大小写敏感**

```bash
git config --global core.ignorecase false
```

**换行符**

- 常用配置（跨平台开发）

    ```bash
    # Windows   仓库使用 LF, 本地使用 CRLF
    git config --global core.aurocrlf true
    # Unix      仓库和本地都使用 LF
    git config --global core.aurocrlf input
    ```

- 基本概念
  - 标准化：在提交代码时，将文本文件中的换行符 `CRLF` 转换成 `LF` 的过程
  - 平台化：在检出代码时，将文本文件中的换行符 `LF` 转换成 `CRLF` 的过程
- 相关配置

    ```bash
    # 控制 Git 是否进行 标准化和平台化 的行为
    git config --global core.autocrlf  [true | input | false]
        true    自动完成标准化与平台化          x -> LF -> CRLF
        input   只做标准化，不做平台化          x -> LF -> LF
        false   保持文件原始的换行符（默认配置） x -> x  -> x

    # 控制 Git 是否阻止添加 混合多种换行符的文件
    git config --global core.safecrlf [true | false | warn]
        true    提交混合换行符的文本文件时进行拦截，提示异常并阻止 git add 操作
        warn    提交混合换行符的文本文件时发出警告，但是不阻止 git add 操作
        false   提交混合换行符的文本文件时没有相关操作（默认配置）
    ```