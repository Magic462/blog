---
title: 百度页面奔跑的白熊
date: 2024-05-16 13:51:58
updated: 2024-05-18 13:14:09
categories: css
tags: [css, css动画，页面练习]
---
# 一、相关知识-动画

## 1.基本使用：先定义再调用

## 2.动画使用步骤

## 调用动画

```css
@keyframes动画名称{
    0%{
        width:100px；
    }
    100%{
        width:200px;
    }
}
```

## 使用动画

```css
div {
    width:200px;
    height:200px;
    background-color:aqua;
    margin:100px auto;
    /*调用动画*/
    animation-name:动画名称；
    /*持续时间*/
    animation-duration:持续时间；单位必须为秒
}
```

## 3.动画

### 动画序列

0%是动画的开始，100%是动画的完成。这样的规则就是动画序列。
在@keyframes中规定某项CSS样式，就能创建由当前样式逐渐改为新样式的动画效果。
动画是使元素从一种样式逐渐变化为另一种样式的效果。您可以改变任意多的样式任意多的次数。
请用百分比来规定变化发生的时间，百分号必须为整数，百分比的划分就是时间的分配，或用关键词"from"和"to",等同于0%和100%

### 动画属性

@keyframes:规定动画
animation:所有动画属性的简写属性，除了animation-play-state属性。
**必需**animation-name:规定@keyframes动画的名称。
**必需**animation-duration:规定动画完成一个周期所花费的秒或毫秒，默认是0。
animation-timing-function:规定动画的速度曲线，默认是“ease”。
animation-delay:规定动画何时开始，默认是0。
animation-iteration-count:规定动画被播放的次数，默认是1，还有infinite
animation-direction:规定动画是否在下一周期逆向播放，默认是"normal”,alternatei逆播放
animation-play-state:规定动画是否正在运行或暂停。默认是"running",还有"pause"。简写里不包括这项
animation-fill-mode:规定动画结束后状态，保持forwards 回到起始backwards
**简写：**
动画名称 持续时间 运动曲线 何时开始 播放次数 是否反方向 动画起始或者结束的状态；
animation:myfirst 5s linear 2s ifinite alternate forwards;

# 二、相关案例-奔跑的白熊

## 大致思路

### 1.html结构

放一个父盒子box和一个子盒子b,分别用于装背景山和白熊

### 2.设计css样式：

#### 先设置奔跑的熊.box .b

由于熊为白色，给body背景一个颜色
**注意**：熊一共走了8步，图片长为1600px，所以添加背景图片时只显示一个熊的片段长为200px
**让熊在原地跑**：
定义bear动画：一共8步，每一步0.8s，无限重复跑步动作
调用bear动画：初始状态为step1，末尾状态为step8,整个背景图片往左移，所以背景移动值为负
**再让熊跑到视窗中央：**
定义move动画：走2.5s，走到终点停止
调用move动画：初始状态在最左侧，末尾状态在屏幕中央，先移动left一半，再移动熊所在盒子（版心）本身的一半即可到中间
同时调用两个动画实现跑到中间的熊

#### 再设置移动的山背景

**注意**由于山背景的宽度为3840px，超出视窗宽度，所以我们给山盒子加宽度时为100%，加背景图片时要用repeat,否则会出现背景有一段为空白

定义mountain动画：走20s,匀速，无限
调用mountain动画：同熊原地跑动画原理相同

我们想让熊覆盖在山上跑动，就要给子盒子加绝对定位、父盒子加相对定位，并给熊加bottom:0,使它在山最下跑
但是此时会发现山和熊在视窗上半部分，给山也加bottom:0,并不能实现，因为山的定位不固定在视窗中，所以要将山的相对定位改为固定定位

## 3.最终完整源码

```css
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        body {
            background-color: #ccc;
        }

        .box {
            bottom: 0;
            width: 100%;
            height: 336px;
            position: fixed;
            background: url(image/bg1\(1\).png) repeat;
            animation: mountain 20s linear infinite;
        }

        .box .b {
            bottom: 0;
            position: absolute;
            width: 200px;
            height: 100px;
            background: url(image/bear\(1\).png) no-repeat;
            animation: bear 0.8s steps(8) infinite, move 2.5s forwards;
        }

        @keyframes bear {
            0% {
                background-position: 0;
            }

            100% {
                background-position: -1600px;
            }
        }

        @keyframes move {
            0% {
                left: 0;
            }

            100% {
                left: 50%;
                margin-left: -100px;
            }
        }

        @keyframes mountain {
            0% {
                background-position: 0;
            }

            100% {
                background-position: -3840px 0;
            }
        }
    </style>
</head>

<body>
    <div class="box">
        <div class="b"></div>
    </div>
</body>

</html>
```