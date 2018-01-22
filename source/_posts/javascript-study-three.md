---
title: Javascript 学习日记三
date: 2018-01-22 09:40:03
tags: 技术
---
# Javascript 学习日记三

## 借助HTML5 details,summary无JS实现各种交互效果

> <http://www.zhangxinxu.com/wordpress/?p=7312>

带slideUp/slideDown效果的多项折叠菜单

首先看下HTML，展开列表结构发生了变化，不是作为<details>的子元素，而是作为其相邻兄弟元素存在，HTML示意：

```html
    <details open><summary>订单中心</summary></details>
    <dl>
        <dd><a href>我的订单</a></dd>
        <dd><a href>我的活动</a></dd>
        <dd><a href>评价晒单</a></dd>
        <dd><a href>购物助手</a></dd>
    </dl>
```

上面<dl>定义列表就是展开收起的内容，其作为兄弟元素和<details>元素平起平坐，于是，我们就可以利用点击<summary>元素<details>元素的open属性会变化的特性实现我们想要的动画效果，CSS如下：

```css
    details + dl {
        max-height: 0;
        transition: max-height .25s;
        overflow: hidden;
    }
    [open] + dl {
        max-height: 100px;
    }
```

效果见
> <http://www.zhangxinxu.com/study/201801/html-details-summary-example-slideup-slidedown.html>

## CSS :focus-within伪类选择器及纯CSS下拉等应用举例

> <http://www.zhangxinxu.com/wordpress/?p=7327>

### 表单输入勿扰模式

```css
    form {
        outline: 2000px solid hsla(0,0%,100%,0);
        transition: outline .2s;
        position: relative;
        z-index: 1;
    }
    form:focus-within {
        outline: 2000px solid hsla(0,0%,100%,1);
    }
```

效果见
> <http://www.zhangxinxu.com/study/201801/focus-within-form-dnd-mode.html>

### 带计数文本域的focus高亮

```css
    .textarea-x:focus-within {
        border-color: #00a5e0;
    }
```

效果见
> <http://www.zhangxinxu.com/study/201801/focus-within-textarea-highlight.html>

### 下拉菜单

```html
    <div class="details">
        <a href="javascript:" class="summary">我的消息</a> 
        <div class="box">
            <a href="javascript:">我的回答<sup>12</sup></a>
            <a href="javascript:">我的私信</a>
            <a href="javascript:">未评价订单<sup>2</sup></a>
            <a href="javascript:">我的关注</a>
        </div>
    </div>
```

```css
    .box {
        display: none;
    }
    .details:focus-within .summary {
        background-color: #fff;
    }
    .details:focus-within .box {
        display: block;
    }
```

效果见
> <http://www.zhangxinxu.com/study/201801/focus-within-pure-css-droplist.html>