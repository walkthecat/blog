---
title: 使用java碰到的问题汇总
date: 2017-07-21 16:13:23
tags: 技术
---

### 1. missing builder
{% asset_img 1.png error image %}

由于是同事使用的是myeclipse创建的项目，导入eclipse后需要把相关的missing builder全部勾掉。

### 2. WARNING: Unknown version string [3.1]. Default version will be used

tomcat 7只支持到最高Servlet3.0，tomcat 8支持servlet3.1 