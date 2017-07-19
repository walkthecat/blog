---
title: hexo使用方法
date: 2017-07-19 15:58:48
tags: 技术
---

## 使用hexo部署Github Pages

>https://gist.github.com/btfak/18938572f5df000ebe06fbd1872e4e39

- [https://github.com/hexojs/hexo](https://github.com/hexojs/hexo)
- [https://hexo.io/docs/](https://hexo.io/docs/)

### 1. 安装 Hexo

    npm install hexo-cli -g

### 2. 建立你的GitHub Pages项目
```
hexo init yourname.github.io 
cd yourname.github.io
npm install
```

### 3. 本地测试你的页面

    hexo server

<!--more-->
### 4. 博客配置设定

```
打开_config.yml

~~~~~~~~~~~~~~~~~~ _config.yml ~~~~~~~~~~~~~~~~~~
# Site
title: yourname's note
subtitle:
description: yourname's personal blog
author: yourname
language:
timezone: PRC

# URL
## If your site is put in a subdirectory, set url as 'http://yoursite.com/child' and root as '/child/'
url: http://yourname.github.io/
root: /
permalink: :year/:month/:day/:title/
permalink_defaults:
```

### 5. 设定Git相关信息

```
npm install hexo-deployer-git --save
打开 _config.yml

~~~~~~~~~~~~~~~~~~ _config.yml ~~~~~~~~~~~~~~~~~~
# Deployment
## Docs: http://hexo.io/docs/deployment.html
deploy:
  type: git
  repo: https://github.com/yourname/***.git
  branch: master
```

### 6. 使用“watch”参数生成文件

"watch" command can monitor your files.
[https://hexo.io/docs/generating.html](https://hexo.io/docs/generating.html)

    hexo generate --watch

### 7. 创建新文章

    hexo new first-post
    INFO  Created: ~/***/yourname.github.io/source/_posts/first-post.md

### 8. 部署你的博客

[https://hexo.io/docs/deployment.html](https://hexo.io/docs/deployment.html)
```
hexo clean
hexo deploy/hexo d
```

### 9. 更换博客皮肤

[https://github.com/hexojs/hexo/wiki/Themes](https://github.com/hexojs/hexo/wiki/Themes)

```
For instance, How to use the following theme.
https://hexo.io/hexo-theme-light/

## Install it
$ cd yourname.github.io
$ git clone git://github.com/tommy351/hexo-theme-light.git themes/light

## Update the above files
$ themes/light
$ git pull

## Set information to use the theme
$ cd yt8yt.github.io
$ vi _config.yml

~~~~~~~~~~~~~~~~~~ _config.yml ~~~~~~~~~~~~~~~~~~
# Extensions
## Plugins: http://hexo.io/plugins/
## Themes: http://hexo.io/themes/
theme: light
```

### 10. 创建新页面

[https://hexo.io/docs/writing.html](https://hexo.io/docs/writing.html)

```
$ hexo new page aboutme
INFO  Created: ~/***/yourname.github.io/source/aboutme/index.md

$ cd source/aboutme/

$ vi index.md
```

### 问题点

如果部署时发生如下错误
{% asset_img error.png error image %}

>https://www.zhihu.com/question/38219432

- 删除你hexo 下面的.deploy_git文件夹
- 运行 
{% asset_img 2.png edit image %}
- 重新 
```
hexo clean
hexo g
hexo d
```
