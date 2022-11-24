---
title: git常用命令步骤
date: 2022-11-23 20:13:48
tags:
---
### 1. 克隆仓库
`git clone 仓库地址`

### 2. 创建自己的分支
`git branch 分支名字`

### 查看当前分支
`git branch -v`

### 查看已有分支
`git branch`

### 删除分支
`git branch -d 分支名字`

### 3. 将文件添加到缓存 (. 代表全部添加)
`git add .`
### 4. 提交到版本库
`git commit -m "提交的描述信息"`

### 5. 将本地内容推送到远程仓库
`git push origin 分支名字`

### -u 若当前分支与多个主机存在追踪关系
`git push -u origin 分支名字`

### -f 强制将本地的修改覆盖了远程仓库的版本，将其他人的 commit 都冲掉，而且无法使用通常的 git reset 方式回退
`git push -u origin 分支名字 -f`

### 6. 切换分支
`git checkout 分支名字`

### 7. 合并分支 （区别：merge 会多出一次 merge commit，rebase 不会，merge 的提交树是非线性的，rebase 的提交树是线性的（通过重写提交历史）；merge 在分支上都是好几条弯弯曲曲的样子，rebase 就一条直线）
`git merge 分支名字`
`git rebase 分支名字`

### 8. 拉取远程仓库代码
`git pull origin 分支名字`

### 其他:
### 关联远程仓库地址：
`git remote add origin http://xxxx@git`

### 初始化仓库:
`git init`