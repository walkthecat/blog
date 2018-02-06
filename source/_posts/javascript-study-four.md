---
title: Javascript 学习日记四
date: 2018-02-06 09:00:03
tags: 技术
---
# Javascript 学习日记四

## CSS content换行技术实现字符animation loading效果

> <http://www.zhangxinxu.com/wordpress/?p=5753>

```css
dot {
    display: inline-block; 
    height: 1em; line-height: 1;
    text-align: left;
    vertical-align: -.25em;
    overflow: hidden;
}
dot::before {
    display: block;
    content: '...\A..\A.';
    white-space: pre-wrap;   /* 也可以是white-space: pre */
    animation: dot 3s infinite step-start both;
}
@keyframes dot {
    33% { transform: translateY(-2em); }
    66% { transform: translateY(-1em); }
}
```

```html
    订单提交中<dot>...</dot>
```

命令行中常见的loading动画实现只要content设置为下面这个值

```css
.loading::after {
  display: inline-table;
  content: "/\A–\A\\\A|";
  white-space: pre;
  animation: spin4 2s steps(4) infinite;
}
```