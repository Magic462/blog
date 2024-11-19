---
title: 会展开的魔方和交错的小块
date: 2024-07-01 15:51:58
updated: 2024-07-02 13:14:09
categories: css
tags: [css, css动画，页面练习]
---
# 一、交错的小块

## 1.大致思路

### 1.基本结构
构建父级正方形盒子后分别在左上角和右下角放两个占父级盒子25%的小盒子，用子绝父相的定位方式将子盒子固定在父盒子中

### 2.实现动态移动
因为两个盒子移动的方式不同，所以分别设置动画属性

**注意**  x轴正方向水平向右，y轴正方向为垂直向下，移动时要加单位，四条边移动四次，一次移动25%

**盒子1**：以左上角为原点，移到右上角坐标为(200，0)，移到右下角坐标为(200,200)，移到左下角坐标为(0，200)，最后回到原点

```css
@keyframes move1 {
    0%{
        transform: translate(0,0);
    }
    25%{
        transform: translate(200px,0);
    }
    50%{
        transform: translate(200px,200px);
    }
    75%{
        transform: translate(0,200px);
    }
    100%{
        transform: translate(0,0);
    }
}
```
**盒子2**
以右下角为原点，移到左下角坐标为(-200，0)，移到左上角坐标为(-200,-200)，移到右上角坐标为(0，-200)，最后回到原点

```css
@keyframes move2 {
    0%{
        transform: translate(0,0);
    }
    25%{
        transform: translate(-200px,0);
    }
    50%{
        transform: translate(-200px,-200px);
    }
    75%{
        transform: translate(0,-200px);
    }
    100%{
        transform: translate(0,0);
    }
}
```

## 2.最终版代码
html

```css
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <link rel="stylesheet" href="style1.css">
</head>
<body>
        <div class="box">
            <div class="box_l"></div>
            <div class="box_r"></div>
        </div>
</body>
</html>
```
css

```css
* {
    padding:0;
    margin:0;
}
.box {
    width: 400px;
    height: 400px;
    margin:100px auto;
    position: relative;
}
.box .box_l {
    position: absolute;
    top:0;
    left:0;
    background-color: pink;
    width: 200px;
    height: 200px;
    animation:move1 5s infinite;
}
.box .box_r {
    position: absolute;
    bottom:0;
    right:0;
    background-color: bisque;
    width: 200px;
    height: 200px;
    animation:move2 5s infinite;
}
@keyframes move1 {
    0%{
        transform: translate(0,0);
    }
    25%{
        transform: translate(200px,0);
    }
    50%{
        transform: translate(200px,200px);
    }
    75%{
        transform: translate(0,200px);
    }
    100%{
        transform: translate(0,0);
    }
}
@keyframes move2 {
    0%{
        transform: translate(0,0);
    }
    25%{
        transform: translate(-200px,0);
    }
    50%{
        transform: translate(-200px,-200px);
    }
    75%{
        transform: translate(0,-200px);
    }
    100%{
        transform: translate(0,0);
    }
}
```
# 二、会展开的魔方

### 1.基本结构;
放一个大盒子container用于装魔方，**记得最后加3d效果**，大盒子中放六个盒子为魔方的六个面，加绝对定位将六个面固定在一起

### 2.静态魔方的构建

**注意：**
**所有的面**都是朝z轴方向移动面的一半距离（75px）,即与人眼的距离拉近75px,并且当鼠标经过时距离拉近200px，**特别注意**当设置各个面时**虽然**普遍情况下我们应该先写移动的距离再旋转，**但是**对于魔方来说，先移动再旋转不会构成封闭的立方体

前面1：

```css
.container div:nth-child(1) {
    transform: translateZ(75px);
}

.container:hover div:nth-child(1) {
    transform: translateZ(200px);
}
```

右面2：

```css
.container div:nth-child(2) {
    transform: rotateY(90deg) translateZ(75px);
}

.container:hover div:nth-child(2) {
    transform: rotateY(90deg) translateZ(200px);
}
```

后面3：

```css
.container div:nth-child(2) {
    transform: rotateY(180deg) translateZ(75px);
}

.container:hover div:nth-child(2) {
    transform: rotateY(180deg) translateZ(200px);
}
```

左面4;

```css
.container div:nth-child(2) {
    transform: rotateY(270deg) translateZ(75px);
}

.container:hover div:nth-child(2) {
    transform: rotateY(270deg) translateZ(200px);
}
```

上面5：

```css
.container div:nth-child(5) {
    transform: rotateX(90deg) translateZ(75px);
}

.container:hover div:nth-child(5) {
    transform: rotateX(90deg) translateZ(200px);
}
```

下面6:

```css
.container div:nth-child(5) {
    transform: rotateX(-90deg) translateZ(75px);
}

.container:hover div:nth-child(5) {
    transform: rotateX(-90deg) translateZ(200px);
}
```

### 3.让静态的魔方动起来
由于魔方的六个面都在大盒子container里，所以只需让大盒子旋转即可实现效果，我们使用动画的属性让魔方动起来，从初始0度到360度让他转一圈，如果x,y轴都写的话就是沿着xOy平面的角平分线旋转

```css
@keyframes rotate {
    0% {
        transform: rotateX(0) rotateY(0);
    }

    100% {
        transform: rotateX(360deg) rotateY(360deg);
    }
}

.container {
    width: 150px;
    height: 150px;
    transform-style: preserve-3d;
    position: relative;
    animation: rotate 5s infinite linear;
    margin: 250px 250px;
}
```

## 2.最终版代码
**html**
```css
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <div class="container">
        <div>a</div>
        <div>a</div>
        <div>a</div>
        <div>a</div>
        <div>a</div>
        <div>a</div>
    </div>
</body>
</html>
```
**css**

```css
* {
    margin: 0;
    padding: 0;
}

body {
    width: 150px;
    height: 150px;
    perspective: 1000px;/*实现近大远小的效果*/
    background: #000;
}

@keyframes rotate {
    0% {
        transform: rotateX(0) rotateY(0);
    }

    100% {
        transform: rotateX(360deg) rotateY(360deg);
    }
}

.container {
    width: 150px;
    height: 150px;
    transform-style: preserve-3d;
    position: relative;
    animation: rotate 5s infinite linear;
    margin: 250px 250px;
}

.container:hover {
    transform: rotateY(180deg) rotateX(180deg);
}/*也可以不写这个hover,它的目的只是更美观*/

.container div {
    width: 100%;
    height: 150px;
    position: absolute;
    background-color: #ccc;
}

.container div:nth-child(1) {
    transform: translateZ(75px);
}

.container:hover div:nth-child(1) {
    transform: translateZ(200px);
}

.container div:nth-child(2) {
    transform: rotateY(90deg) translateZ(75px);
}

.container:hover div:nth-child(2) {
    transform: rotateY(90deg) translateZ(200px);
}

.container div:nth-child(3) {
    transform: rotateY(180deg) translateZ(75px);
}

.container:hover div:nth-child(3) {
    transform: rotateY(180deg) translateZ(200px);
}

.container div:nth-child(4) {
    transform: rotateY(270deg) translateZ(75px);
}

.container:hover div:nth-child(4) {
    transform: rotateY(270deg) translateZ(200px);
}

.container div:nth-child(5) {
    transform: rotateX(90deg) translateZ(75px);
}

.container:hover div:nth-child(5) {
    transform: rotateX(90deg) translateZ(200px);
}

.container div:nth-child(6) {
    transform: rotateX(-90deg) translateZ(75px);
}

.container:hover div:nth-child(6) {
    transform: rotateX(-90deg) translateZ(200px);
}
```