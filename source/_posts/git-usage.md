---
title: git用法
tags: 技术
---

### git使用方法

初始化仓库
    
    git init

添加readme

    git add README.md
    
提交一次

    git commit -m "first commit"
    
添加源
    
    git remote add origin  https://github.com/walkthecat/testgit
    
推送到

    git push -u origin master

删除远程目录且不删除本地文件

>https://stackoverflow.com/questions/7927230/remove-directory-from-remote-repository-after-adding-them-to-gitignore

```
git rm -r --cached your-directory
git commit -m "Remove the now ignored directory your-directory"
```