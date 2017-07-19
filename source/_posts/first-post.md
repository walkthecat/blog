---
title: 创建gh-pages项目
tags: 技术
---

## 创建项目

    mkdir 目录名

对该目录进行git初始化。

    cd 目录名
    git init

创建一个没有父节点的分支gh-pages。

    git checkout --orphan gh-pages

## 发布内容

把所有内容加入本地git库。

    git add .
    git commit -m "first post"

前往github的网站，在网站上创建一个名为前面所建目录名的库。接着，再将本地内容推送到github上你刚创建的库。注意，下面命令中的username，要替换成你的username。

    git remote add origin https://github.com/username/xxx.git
    git push origin gh-pages