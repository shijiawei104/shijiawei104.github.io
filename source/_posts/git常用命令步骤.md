---
title: git常用命令步骤
date: 2022-11-23 20:13:48
categories:
- git
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

### 本地项目推送到两个Git地址
方法一：添加另外一个git地址
`git remote add origin2 http://xx.git    // origin2可以自定义`
拉取另外一个git地址上的数据源
`git pull origin2 master --allow-unrelated-histories   （--allow-unrelated-histories是为了解决冲突）`
本地项目推送到另外一个地址中
`git push origin2`
每次可以分开提交
`git push`
`git push origin2`
查看此时的包括两个远程地址:git remote -v
删除远程地址:git remote rm origin2
方法二：给origin 增加一个可以push的地址
`git remote set-url --add origin http://xx.git   //给origin添加一个远程push地址，这样一次push就能同时push到两个地址上面`
`git remote -v //查看是否多了一条push地址（这个可不执行）`
`git push origin master -f    // 如果第一次推不上去代码，可以使用强推的方式`
删除添加的路径的方法:`git remote set-url --delete origin http://xx.git`