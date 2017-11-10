---
title: Javascript 学习日记二
date: 2017-11-10 09:40:03
tags: 技术
---
# Javascript 学习日记二

## CSS3 linear-gradient线性渐变实现虚线等简单实用图形

> <http://www.zhangxinxu.com/wordpress/?p=6494>

通过对linear-gradient和background-size（重要）的设置以达到各种效果，background-size可以约束在特定区域以达到效果

示例代码

加减号的实现

```css
.btn-plus {
    background-image: linear-gradient(to top, currentColor, currentColor), linear-gradient(to top, currentColor, currentColor);
    background-size: 10px 2px, 2px 10px;
}
.minus {
    background-image: linear-gradient(to top, #666, #666);
    background-size: 10px 2px;
}
```

dashed虚线的自定义

```css
.dashed {
    height: 1px;
    background: linear-gradient(to right, #000000, #000000 5px, transparent 5px, transparent);
    background-size: 10px 100%;
}
```

虚线三角

```css
.tri {
    width: 6px; height: 6px;
    background: linear-gradient(to top, #ddd, #ddd) no-repeat, linear-gradient(to right, #ddd, #ddd) no-repeat, linear-gradient(135deg, #fff, #fff 6px, hsla(0,0%,100%,0) 6px) no-repeat;
    background-size: 6px 1px, 1px 6px, 6px 6px;
    transform: rotate(-45deg);
}
```

效果见
> <http://www.zhangxinxu.com/study/201710/css3-linear-gradient-dashed-generate.html>

## 3种纯CSS实现中间镂空的12色彩虹渐变圆环方法

> <http://www.zhangxinxu.com/wordpress/?p=6516>

- 借助CSS3 conic-gradient锥形渐变实现12色渐变圆环
- 借助CSS3 linear-gradient线性渐变近似实现12色渐变圆环
- 借助CSS clip剪裁、transform旋转和模糊滤镜实现12色渐变圆环