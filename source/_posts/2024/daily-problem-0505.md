---
title: css基础之盒子模型、浮动问题
date: 2024-05-05 21:03:58
updated: 2024-05-06 13:14:09
categories: css
tags: [css, 学习笔记]
---
# 盒子模型

# 一、盒子模型的组成

border边框、content内容、padding内边距、margin外边距(与另外盒子的距离)

## 1.边框

border-width
border-style: solid实线 border-style: dashed虚线 border-style: dotted点线
border-color

border: 1pxx solid pink;复合写法，无顺序
border-top上边框border-bottom下边框border-left左边框border-right右边框
border-collapse: collapse表示相邻边框合到一起

## 2.内边距padding

如果盒子已经有了大小，再加内边距，盒子会变大
如果没有指定宽度，加内边距，盒子不变
padding-left: 10px (right,top,bottom)
复合写法：padding: 1px;
一个值：上下左右
两个值：上下，左右
三个值：上，下，左右
四个值：上，右，下，左，顺时针

## 3.外边距margin

可以让盒子水平居中但要满足两个条件：
必须指定宽度，左右外边距都设置为auto
常用：magin:0,auto;
注意：让行内元素或行内块元素水平居中只需给父元素添加text-align: center;
塌陷问题：对于嵌套的父子块元素，当父元素有上外边距子元素也有上外边距，父元素会塌陷较大的外边距值
三种解决方法：
给父元素定义上边框
给父元素定义上内边距
为父元素添加overflow:hidden

## 4.清除内外边距(一般是css代码的第一句)

`*` {
    padding: 0;
    margin: 0;
}

**注意**：行内元素为了照顾兼容性尽量只设置左右边距，不要设置上下，但转换为块级或行内块就可以了

# 二、ps基本操作

文件，打开：可以打开要测量的图片
ctrl+R:可以打开标尺（视图+标尺）
ctrl+放大ctrl-缩小
空格键变小手，可以拖动ps视图
选区工具测量大小，空白处点击取消选区

# 三、圆角边框

## 1.圆角边框的原理

border-radius: length;length为四个角圆的半径，值越大，弧线越大

## 2.圆角边框的使用

将正方形改为圆形：
border-radius: 边长*50%px;
将长方形改为椭圆：
border1-radius: 短边*50%px;

可以设置不同的圆角：
border-radius: 左上右上右下左下；从左上开始顺时针
或者：
border-top-left-radius
border-top-right-radius
border-bottom-left-radius
border-bottom-right-radius

# 四、盒子阴影box-shadow

h-shadow:必需，水平阴影，负值左移正值右移
v-shadow:必需，垂直阴影，负值上移，正值下移
blur:模糊距离（为0时影子为实）
spread:阴影的尺寸
color：一般为rgba(0,0,0,0.3)30%的黑色
insert:将外阴影改为内阴影

注意：盒子阴影不占用空间，不影响排列
使鼠标经停盒子才出现阴影

```css
<style>
    div:hover {
        box-shadow:10px 10px 10px -4px rgba(0,0,0,0.3);
    }
</style>
```

# 五、文字阴影text-shadow

h-shadow:必需，水平阴影，负值左移正值右移
v-shadow:必需，垂直阴影，负值上移，正值下移
blur:模糊距离
color：一般为rgba(0,0,0,0.3)30%的黑色

# 浮动问题

# 一、浮动基本定义

## 1.定义

float属性用于创建浮动窗，将其移动一边，直到左边缘或右边缘触及包含块或另一个浮动窗的边缘

## 2.语法

选择器{
    float:属性；
}
属性：none不浮动,left向左浮动,right向右浮动;

# 二、浮动特性

## 1.最重要特性

脱标：脱离标准普通流的控制移动到指定位置
浮动的盒子不再保留原先的位置

## 2.如果多个盒子都加了浮动特性，那么所有盒子都在一行内显示，没有空隙，且顶端对齐

## 3.浮动元素具有行内块元素特性

任何元素在添加浮动特性都可以浮动
注意：如果块级元素没有设置宽度，默认宽度和父级一样宽，但是添加浮动后，它的大小根据内容决定

## 4.注意：

浮动元素经常和标准流父级搭配使用：先用标准流的父级排列上下位置，再用子级采用浮动排列左右位置
一个元素浮动，理论上其它兄弟元素跟着浮动：浮动的盒子只会影响后面的标准流

## 5.为什么要清除浮动

当子盒子元素较多，或文字较多，不方便给父盒子的高度时，我们想要有多少子元素就撑开父元素

## 6.清除浮动本质

清除浮动的本质是清除浮动元素的影响
如果父盒子本身有高度，则不需要清除浮动
清除浮动之后，父级会根据浮动的子盒子自动检测高度，父级有了高度就不会影响下面的标准流
选择器{clear:属性值；}
属性值：left,right,both(几乎只用both)

## 7.清除浮动的方法

额外标签法：在最后一个浮动盒子后添加一个空的块级标签(或者`</br>`)
例如：`<div class="qingchu"> </div>,调用.qingchu{clear:both;}`
父级添加overflow属性：将其属性值设置为hidden或auto或scroll
父级添加after伪元素

```css
.clearfix:after{
    content: "";
    display: block;
    height: 0;
    clear: both;
    visibility: hidden;
}
.clearfix {
    *zoom: 1;/*zoom照顾I6I7低版本的浏览器*/
}
```

父级添加双伪元素

```css
.clearfix:before,.clearfix:after{
    content: "";
    display: table;
}
.clearfix {
    clear:both;
}
.clearfix {
    *zoom: 1;
}
```
