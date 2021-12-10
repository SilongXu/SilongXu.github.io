---
title: 项目协作时git管理代码命令
categories: 基础配置
tags: [git]
abbrlink: 'gitManage'
---

# 项目协作时git管理代码的命令

### 提交上传代码的命令

| **git add -A**             | **代码存入缓存区**                                           |
| -------------------------- | :----------------------------------------------------------- |
| **git commit -m "注释"**   | **提交代码到本地仓库**                                       |
| **git commit --amend**     | **提交代码，且本次提交的代码和上次提交的代码进行合并**       |
| **git rebase -i HEAD~x**   | **这里的x填数字，意思是显示前x次数的提交。通常用来进行commit合并或者删除** |
| **git push origin 分支名** | **将代码从本地仓库提交到远端仓库**                           |

### 拉取代码的命令

| git add -A                   | 代码存入暂存区                 |
| :--------------------------- | :----------------------------- |
| **git commit -m ""**         | **提交代码到本地仓库**         |
| **git fetch origin**         | **从远端拿到代码**             |
| **git rebase origin/master** | **远端代码和本地代码进行比较** |
| **git add -A**               | **比较完毕之后进行提交**       |
| **git rebase --continue**    | **看比较是否成功（没有遗漏）** |

### 分支开发

协作开发的时候不要在master上进行开发，要在分支开发，完成后再进行master合并

| git checkout master            | 切换到master分支                     |
| ------------------------------ | ------------------------------------ |
| **git rebase origin**          | **查看远端的master的代码**           |
| **git pull**                   | **拉取代码**                         |
| **git checkout -b branchName** | **创建并切换到一个新的分支**         |
| git branch                     | **查看所有分支，并显示当前所在分支** |

### 分支落后时如何处理

| git checkout master                     | 切换到master分支                   |
| --------------------------------------- | ---------------------------------- |
| **git pull --rebase**                   | **拉取master的代码并且进行rebase然后切回开发分支** |
| **git add -A**                          | **冲突处理完毕后，保存到暂存区**   |
| **git commit --amend**                  | **提交冲突处理完的代码**           |
| **git push origin featureName --force** | **然后强推到分支中**               |

tips:合并了的commit 就不要再--amend了，需要重新-m一个commit