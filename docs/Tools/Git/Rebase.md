# Git - Rebase

## 重构最近的一次提交

### 修改提交信息

```bash
git commit -m "rebase latest commit msg" --date="2019-01-04T12:00:00+0800" --author="epoch <epoch@gmail.com>"
```

### 修改提交内容

1. 直接修改文件内容并添加到暂存区
2. 合并内容到上一次提交

   ```bash
   git commit -m "rebase latest commit content & msg"
   ```

## 重构之前的多个提交

- 交互式重构命令

    ```bash
    # 从指定的提交之后开始进行重构
    git rebase -i <revision> # revison: commit hash
    ```

- 选项功能介绍

    ```bash
    # Commands:
    # p, pick <commit> = use commit
    # r, reword <commit> = use commit, but edit the commit message
    # e, edit <commit> = use commit, but stop for amending
    # s, squash <commit> = use commit, but meld into previous commit
    # f, fixup <commit> = like "squash", but discard this commit's log message
    # x, exec <command> = run command (the rest of the line) using shell
    # b, break = stop here (continue rebase later with 'git rebase --continue')
    # d, drop <commit> = remove commit
    # l, label <label> = label current HEAD with a name
    # t, reset <label> = reset HEAD to a label
    # m, merge [-C <commit> | -c <commit>] <label> [# <oneline>]
    # .       create a merge commit using the original merge commit's
    # .       message (or the oneline, if no original merge commit was
    # .       specified). Use -c <commit> to reword the commit message.
    ```

## 其他重构场景

### 批量修改提交作者信息

```bash
# 修改作者名称
git filter-branch -f --env-filter 'export GIT_AUTHOR_NAME=epochwz;export GIT_COMMITTER_NAME=epochwz' --tag-name-filter cat -- --branches --tags
```

## 学习资源

[为什么？还有怎样才能保持你的 Git 提交历史清晰？](https://learnku.com/laravel/t/13616/why-how-can-you-keep-your-git-history-clear)