---
date: 2017-12-05T15:36:35.000Z
title: Git(叁)|分支管理篇
tags:
  - Tools
  - Git
categories:
  - Tools
  - Git
---

## 分支(Branch)

HEAD 指向当前分支的最新提交

**创建并切换到新分支**

```
git checkout -b <new_branch>
```

**查看当前分支**

```
git branch
```

**切换分支**

```
git checkout <current_branch>
```

**合并指定分支到当前分支**

```
git merge <merge_branch>
```

**删除无用的分支**

```
git branch -d <merge_branch>
# 强制删除未合并的分支
git branch -D branch
```

**合并冲突**

```
1. 当其他分支和master分支修改并提交了同一个文件时，合并分支时就会产生冲突，Git无法使用Fast-Forward模式合并，需要手动修改冲突文件来解决冲突。

2. 修改冲突后，执行git add <filename>以标记为合并成功并Commit本次合并
```

**禁用Fast-Forward**

```
git merge --no-ff -m "commit message" <merge_branch>
```



## 标签

```
# 查看当前的分支
git branch
# 切换到要打标签的分支上
git check branch
# 打标签
git tag v1.0
# 查看标签
git tag
# 打之前commit的标签
git tag v0.9 commit-id
# 查看标签信息
git show v0.9
# 创建待说明信息的标签-a指定标签名,-m 指定标签信息
git tag -a v0.1 -m "message"

# 删除标签
git tag -d v0.1
# 推送标签到远程仓库
git push origin tagName
# 推送全部标签
git push origin --tags

# 删除远程标签
git tag -d tagName
git push origin :refs/tags/tagName
```

## 分支管理

* `master` 稳定的分支，仅用于发布新版本
* `dev` 不稳定的分支，平时开发用的分支。直到稳定版本时，将该分支合并到`master` 分支进行版本的发布
* 每个人都在个人分支上干活，时不时的往`dev` 分支合并
* `bug` 专门用于修复bug的临时分支
* `feature` 分支用于开发新功能



