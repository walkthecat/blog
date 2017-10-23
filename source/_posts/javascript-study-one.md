---
title: Javascript 学习日记一
date: 2017-10-23 10:55:03
tags: 技术
---
# Javascript 学习日记一

## 鼠标无限移动 JS API Pointer Lock

> <http://www.zhangxinxu.com/wordpress/?p=6473>

Pointer Lock API可以使鼠标无限移动，坐标无限增大。

示例代码

html部分：

```html
    <img id="image" src="mm1.jpg">
```

JS部分：

```javascript
var eleImage = document.getElementById('image');
if (eleImage) {
    // 起始值
    var moveX = 0, moveY = 0;
    // 图片无限变换的方法
    var rotate3D = function (event) {
        moveX = moveX + event.movementX;
        moveY = moveY + event.movementY;

        eleImage.style.transform = 'rotateX(' + moveY + 'deg) rotateY(' + moveX + 'deg)';
    };

    // 触发鼠标锁定
    eleImage.addEventListener('click', function () {
        eleImage.requestPointerLock();
    });

    // 再次点击页面，取消鼠标锁定处理
    document.addEventListener('click', function () {
        if (document.pointerLockElement == eleImage) {
            document.exitPointerLock();
        }
    });

    // 检测鼠标锁定状态变化
    document.addEventListener('pointerlockchange', function () {
        if (document.pointerLockElement == eleImage) {
            document.addEventListener("mousemove", rotate3D, false);
        } else {
            document.removeEventListener("mousemove", rotate3D, false);
        }
    }, false);
}
```

效果见
> <http://www.zhangxinxu.com/study/201709/js-pointer-lock-demo.html>

<!--more-->

## Chrome opacity非1时border-radius圆角边框剪裁问题

> <http://www.zhangxinxu.com/wordpress/?p=6482>

导致圆角边框出现剪裁问题的原因有两个：

- 元素的透明度opacity不是1；
- 元素的位置并不是完美起止于屏幕的像素点上；

解决方法为：

- 使用RGBA或者HSLA颜色代替opacity

```css
border: 1px solid hsla(0,0%,100%,.6);
border: 1px solid rgba(255,255,255,.6);
```

## 借助CSS Shapes实现元素滚动自动环绕iPhone X的刘海

> <http://www.zhangxinxu.com/wordpress/?p=6426>

环绕齐刘海滚动实现原理:

通过使用CSS Shapes中的shape-outside属性，可以让内联元素以不规则的形状进行外部排列。

```css
/* 关键字值 */
shape-outside: none;
shape-outside: margin-box;
shape-outside: content-box;
shape-outside: border-box;
shape-outside: padding-box;

/* 函数值 */
shape-outside: circle();
shape-outside: ellipse();
shape-outside: inset(10px 10px 10px 10px);
shape-outside: polygon(10px 10px, 20px 20px, 30px 30px);

/* <url>值 */
shape-outside: url(image.png);

/* 渐变值 */
shape-outside: linear-gradient(45deg, rgba(255, 255, 255, 0) 150px, red 150px);
```

shape-outside属性要想生效，本身需要是浮动float元素。（静态效果代码）

```css
.shape {
  float: left;
  shape-outside: polygon(0 0, 0 150px, 16px 154px, 30px 166px, 30px 314px, 16px 326px, 0 330px, 0 0);
}
```

动态效果代码

```javascript
box.addEventListener('scroll', function () {
  var scrollTop = box.scrollTop;
  // 滚动偏移应用在shape-outside上
  shape.style.shapeOutside = 'polygon(0 0, 0 '+ (150 + scrollTop) +'px, 16px '+ (154 + scrollTop) +'px, 30px '+ (166 + scrollTop) +'px, 30px '+ (314 + scrollTop) +'px, 16px '+ (326 + scrollTop) +'px, 0 '+ (330 + scrollTop) +'px, 0 0)';
});
```

效果预览

> [demo页面](<http://www.zhangxinxu.com/study/201709/css3-shapes-around-iphone-x.html>)

还可以使用环绕图片方式实现

```javascript
box.addEventListener('scroll', function () {
  var scrollTop = box.scrollTop;
  // 滚动偏移应用在margin-top上
  shape.style.marginTop = (150 + scrollTop) + 'px';
});
}
```

```css
.shape {
  float: left;
  shape-outside: url(liu-outside.png);
  margin-top: 150px;
}
```

效果预览

> [shape-outside url实现列表环绕iPhone X刘海demo](http://www.zhangxinxu.com/study/201709/css3-shapes-url-for-iphone-x.html)

## 连续数字数值转换成逗号分隔数值表示

- 使用正则表达式

```javascript
String(123456789).replace(/(\d)(?=(\d{3})+$)/g, "$1,");
```

- 使用toLocaleString()方法

```javascript
(123456789).toLocaleString('en-US');
```

如果想对逗号分隔符进行修改可以使用CSS中的unicode-range属性,参见：

> [CSS unicode-range特定字符与自定义字体](http://www.zhangxinxu.com/wordpress/2016/11/css-unicode-range-character-font-face/)

## 使用canvas实现和HTML5 video交互的弹幕效果

> <http://www.zhangxinxu.com/wordpress/?p=6386>

## CSS百分比padding实现比例固定图片自适应布局

> <http://www.zhangxinxu.com/wordpress/?p=6348>

图片宽度100%的容器，高度按比例的场景，padding-bottom的百分比值大小就是图片元素的高宽比。

```html
<div class="works-item-t">
    <img src="./150x200.png">
</div>
```

```css
.works-item-t {
    padding-bottom: 133%;
    position: relative;
}
.works-item-t > img {
    position: absolute;
    width: 100%; height: 100%;
}
```

但，有时候，图片宽度并不是100%容器的，例如，图片宽度50%容器宽度，图片高宽比4:3，此时，CSS垂直方向百分比就666了，如下：

```css
.img-box {
    padding: 0 50% 66.66% 0;
}
```

效果预览

> [需要保持图片比例且宽度自适应padding实现demo](http://www.zhangxinxu.com/study/201708/percent-padding-auto-layout.html)