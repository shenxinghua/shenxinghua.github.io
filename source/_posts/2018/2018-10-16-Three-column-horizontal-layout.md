---
title: 如何实现三栏水平布局
date: 2018-10-16
datestring: "20181016"
pageid: 1
categories: 
- 前端
tags:
- CSS
photos: /20181016/1/flower.jpg
summary: 如何实现一个三栏水平布局，其中左边和右边分别位于两侧且宽度固定，中间部分要求宽度自适应？
---

面试时经常会出这样一个问题：如何实现一个三栏水平布局，其中左边和右边分别位于两侧且宽度固定，中间部分要求宽度自适应。

这个问题在平时开发中也经常会遇到。开发者们一般会选择自己熟悉的方法，但能有多少种实现的方法？我总结了一下，可以有四种方法来实现。

## 方法一：绝对定位的方法
这个方法是我最常用的方法。由于绝对定位的元素会脱离文档流，因此不会占据文档流的空间。无代码就如同耍流氓，先上代码：

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width">
  <title>绝对定位法</title>
  <style>
    .container{
      position: relative;
    }

    .container>div{
      height: 100px;
    }

    .left{
      position: absolute;
      left: 0;
      top:0;
      width:200px;
      background: #f00;
    }
    .right{
      position: absolute;
      right: 0;
      top:0;
      width:200px;
      background: #0f0;
    }

    .main{
      padding: 0 200px;
      background: #ff0
    }
  </style>
</head>
<body>
  <div class="container">
    <div class="main">main</div>
    <div class="left">left</div>
    <div class="right">right</div>
  </div>
</body>
</html>
```

效果如下：

![](477ff62a.png)

<br>

## 方法二： 圣杯布局
圣杯布局是Kevin Cornell在2006年提出的一个布局模型概念，是一种经典的三列水平布局解决方案。其核心的方法是巧妙地运用margin负值和相对定位。 

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width">
  <title>圣杯布局</title>
  <style>
    .container{
      padding: 0 200px 0 200px;
    }

    .container>div{
      height: 100px;
      float: left;
      position: relative;
    }

    .left{
      width:200px;
      left: -200px;
      margin-left: -100%;
      background: #f00;
    }
    
    .right{
      width:200px;
      right: -200px;
      margin-left: -200px;
      background: #0f0;
    }

    .main{
      background: #ff0;
      width: 100%;
    }
  </style>
</head>
<body>
  <div class="container">
    <div class="main">main</div>
    <div class="left">left</div>
    <div class="right">right</div>
  </div>
</body>
</html>
```

<br>

## 方法三： 双飞翼布局

双飞翼布局是由淘宝UED的工程师（传说是玉伯）改进圣杯布局并传播开来。圣杯布局和双飞翼布局解决问题的方案在前一半是相同的，也就是三栏全部float浮动，但左右两栏加上负margin让其跟中间栏div并排，以形成三栏布局。不同在于解决 “中间栏div内容不被遮挡”问题的思路不一样。 

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width">
  <title> 双飞翼布局</title>
    <style type="text/css">
    .container>div{
        float: left;
        height: 100px;
        text-align: center;
    }
    .left{
        width:200px;
        margin-left: -100%;
        background: #f00;
    }

    .right{
        width: 200px;
        margin-left: -200px;
        background: #0f0;
    }
    .main{
        background: #ff0;
        width: 100%;
    }
    .content{
        margin: 0 200px 0 200px;
    }
    </style>
</head>
<body>
  <div class="container"> 
　　<div class="main">
    　　<div class="content">main</div> 
      </div>
      <div class="left">left</div> 
      <div class="right">right</div> 
  </div>
</body>
</html>
```

<br>

## 方法四：Flex布局

Flex 是 Flexible Box 的缩写，意为”弹性布局”，用来为盒状模型提供最大的灵活性。任何一个容器都可以指定为 Flex 布局，所以Flex 布局将成为未来布局的首选方案。用它可以轻松实现前几种方法所实现的效果。

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width">
  <title>flex布局</title>
  <style>
    .container{
        display: flex;
        min-height: 100px;
    }

    .left{
      order: -1;
      flex-basis: 200px;
      background: #f00;
    }
    
    .right{
      flex-basis: 200px;
      background: #0f0;
    }

    .main{
      background: #ff0;
      flex-grow:1;
    }
  </style>
</head>
<body>
  <div class="container">
    <div class="main">main</div>
    <div class="left">left</div>
    <div class="right">right</div>
  </div>
</body>
</html>
```