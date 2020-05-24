---
title: git使用指南
categories:
  - git
tags:
  - git
toc: false
date: 2019-08-01 23:28:07
---

## git使用指南
[如何在本地环境配置github](https://segmentfault.com/a/1190000002533334)
### git创建新目录并提交至远程仓库
```bash
git init
git add README.md
git commit -m "first commit"
git remote add origin https://github.com/vinovos/git-pages-source.git
git push -u origin master
```
<!-- more -->
### git add命令
git add xx命令可以将xx文件添加到暂存区
如果有很多改动可以通过 `git add -A`来一次添加所有改变的文件。
*  git add -A  提交所有变化
*  git add -u  提交被修改(modified)和被删除(deleted)文件，不包括新文件(new)
*  git add .  提交新文件(new)和被修改(modified)文件，不包括被删除(deleted)文件
notes: git版本不同会的命令有不同的解释
[git add -A 和 git add . 的区别](https://blog.csdn.net/leorx01/article/details/72358401)