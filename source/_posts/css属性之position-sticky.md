---
title: 'css属性之position:sticky'
tags: css
categories: 前端
date: 2020-01-02 22:23:49
---


## 回顾position属性其它内容
position一共5中定位方式，即static，relative，absolute，fixed，sticky。

static：默认位置，正常流，元素不重叠。
relative：配合top、bottom、left、right，相对该元素默认位置static进行偏移。
absolute：配合top、bottom、left、right，相对于不为static的父级元素。
fixed：配合top、bottom、left、right，相对于视口(viewport，浏览器窗口)。

<!-- more -->

## sticky
sticky会产生动态效果：元素根据正常文档流进行定位，然后相对它的最近滚动祖先，基于top、bottom、left、right的值进行偏移。

当页面滚动:
1. 父元素开始脱离视口时（即部分不可见），只要与sticky元素的距离达到设定的top、bottom、left、right生效门槛，产生fixed定位的效果，但这个fixed效果是被限制在父元素之内的，并且当父元素逐渐滚动出视口时最大限度尽可能展示该元素，而不一定使用top、bottom、left、right设定的值；
2. 等到父元素完全脱离视口时（即完全不可见），fixed定位效果也随之消失，不会再出现在视口中。

注意：
一个sticky元素会“固定”在离它最近的一个拥有“滚动机制”的祖先上（当该祖先的overflow 是 hidden, scroll, auto, 或 overlay时），即便这个祖先不是真的滚动祖先。这个阻止了所有“sticky”行为。

示例代码：
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
</head>
<body>
  <section>
    <ul></ul>
    <ul>
      <li></li>
    </ul>
    <ul></ul>
    <style>
      * {
        margin: 0;
        padding: 0;
      }

      ul {
        background: skyblue;
        width: 100%;
        height: 2000px;
        border: 5px solid black;
        box-sizing: border-box;
      }

      li {
        background-color: tomato;
        width: 100%;
        height: 100px;

        position: sticky;
        top: 200px;
      }
    </style>
  </section>
</body>
</html>
```