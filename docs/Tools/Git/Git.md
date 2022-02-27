# Git

## 学习资源

- 中文书籍：[Git Documentation - Book - Pro Git](https://git-scm.com/book/zh/v2)
- 在线参考手册：[Git Documentation - Reference Manual](https://git-scm.com/docs)
- 本地参考手册 `git help <topic>`
  - `Git Configuration`: `git help config`

## 常用操作

### 提交

```bash
# 提交：将暂存区的文件提交到历史区
git commit -m "commit with message"
# 将工作区中已跟踪的文件添加到暂存区并提交
git commit -am "stage & commit"
# 在提交的时候指定 提交日期、提交作者等提交信息
git commit -m "commit with info" --date="2019-01-04T12:00:00+0800" --author="epoch <epoch@gmail.com>"
```

### 远程仓库

**远程仓库地址管理**

```bash
# 查看远程仓库地址
git remote -v
# 添加远程仓库地址
git remote add <remote-name> <url>
# 删除远程仓库地址
git remote rm <remote-name>
# 修改远程仓库地址
git remote set-url <remote-name> <url>
```

**关联远程仓库**

```bash
# 推送并关联远程仓库
git push -u <remote-name> <branch>

# 关联远程仓库的指定分支
git branch --set-upstream-to=<remote-name>/<branch> <local-branch>

# 拉取指定远程分支
git fetch <origin> <branch>

# 删除远程分支
git push -d origin <branch> 
# 删除 Tag
git push origin :refs/tags/<tag>
```

**其他功能**

```bash
# 拉取所有远程分支
git branch -r | grep -v '\->' | while read remote; do git branch --track "${remote#origin/}" "$remote";done;git fetch --all
# 清理无效的远程分支（远程分支已删除）
git remote prune origin
# 清理本地版本库 
git gc --prune=now
# 查看 pack 空间使用情况
git count-objects -v
```
