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

## crossOrigin属性解决canvas图片getImageData跨域

> <http://www.zhangxinxu.com/wordpress/?p=7378>

在HTML5中，有些元素提供了支持CORS(Cross-Origin Resource Sharing)（跨域资源共享）的属性，这些元素包括\<img\>，\<video\>，\<script\>等，而提供的属性名就是crossOrigin属性。

```javascript
var canvas = document.createElement('canvas');
var context = canvas.getContext('2d');

var img = new Image();
img.crossOrigin = '';
img.onload = function () {
    context.drawImage(this, 0, 0);
    context.getImageData(0, 0, this.width, this.height);
};
img.src = 'https://avatars3.githubusercontent.com/u/496048?s=120&v=4';';
```

关键处为img.crossOrigin = ''

crossOrigin可以有下面两个值

|关键字|释义|
|:---|:---|
|anonymous|元素的跨域资源请求不需要凭证标志设置。|
|use-credentials|元素的跨域资源请求需要凭证标志设置，意味着该请求需要提供凭证。|