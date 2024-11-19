---
title: js获取、操作对象以及年会抽奖案例
date: 2024-06-25 17:25:29
updated: 2024-06-25 23:14:09
categories: javascript
tags: [javascript]
---
# 基础语法04

# 一、对象

## 1.定义
JavaScript里的一种数据类型，可以理解为无序的数据集合，数组为有序的数据集合

## 2.特点
无序的数据的集合，可以详细的描述某个事物

# 二、对象的使用

## 1.声明
let 对象名 = {} let 对象名=new Object()

## 2.由属性和方法组成
let 对象名={
    属性名:属性值,
    方法名:函数,
}

## 3.属性
属性都是成对出现的，包括属性名和值，它们之间使用英文：分隔
多个属性之间使用英文，分隔
属性就是依附在对象上的变量（外面是变量，对象内是属性）
属性名可以使用"或"，一般情况下省略，除非名称遇到特殊符号如空格、中横线等

## 4.增删改查
查:对象.属性 或 对象名['属性名']单引或双引都可以
改:对象.属性值=值
增:对象名.新属性名=新值
删:delete 对象名.属性名
其中增和改的语法类似，当对象中有这个属性时，为改，当对象没有这个属性时，为增

## 5.方法
```javascript
let person = {
    name:'andy',
    sayHi: function(){
        document.write(hi)
    }
}
```
方法是由方法名和函数两部分构成，它们之间使用：分隔
方法调用时也可以添加形参或实参，即使没有参数也要写(),person.sayHi()
方法是依附在对象中的函数
方法名可以使用"或"，一般情况下省略，除非名称遇到特殊符号如空格、中横线等

# 三、遍历对象
k为字符串，代表对象里带引号的属性名'name'
获得对象属性为k
获得对象值为obj[k]
```javascript
for(let k in obj){
    console.log(obj[k])
}
```

# 四、内置对象
## 1.介绍
Math对象是JavaScript提供的数学对象

## 2.作用
提供了一系列做数学运算的方法

## 3.方法
ceil:向上取整
floor:向下取整
round:四舍五入，对于负数-1.5四舍五入为-1
max:找最大数
min:找最小数
pow:幂运算
abs:绝对值
Math对象在线文档，搜索mdn文档
random:生成0-1之间的随机数（包含0不包括1）

## 注意：
null也是Javascript中数据类型中的一种，通常只用它表示不存在的对象，使用typeof检测类型时结果为object

## 生成任意范围的随机数
如何生成`[N,M]`的随机数`Math.floor(Math.random()*(M-N+1))+N`

# 五、声明变量注意
1. 以后声明变量我们优先使用哪个？
const
有了变量先给const,如果发现它后面是要被修改的，再改为let
2. 为什么consti声明的对象可以修改里面的属性？
因为对象是引用类型，里面存储的是地址，只要地址不变，就不会报错
建议数组和对象使用const来声明
3. 什么时候使用let声明变量？
如果基本数据类型的值或者引用类型的地址发生变化的时候，需要用let
比如一个变量进行加减运算，比如fo循环中的i++

# WebAPI01

# 一、Web API 基本认知
## 1.作用和分类
作用：使用js操作html和浏览器，分类：DOM,BOM

## 2.DOM
DOM(Document Object Model-一文档对象模型)是用来呈现以及与任意HTML或XML文档交互的API
白话文：DOM是浏览器提供的一套专门用来操作网页内容的功能
作用：操作网页内容，可以开发网页内容特效和实现用户交互

## 3.DOM树
将HTML文档以树状结构直观的表现出来，我们称之为文档树或DOM树
描述网页内容关系的名词
作用：文档树直观的体现了标签与标签之间的关系

## 4.DOM对象
定义：浏览器根据html标签生成的js对象
核心思想：把网页内容当做对象来处理
document对象
是DOM里提供的一个对象
所以它提供的属性和方法都是用来访问和操作网页内容的
例：document.write()
网页所有内容都在document里面

# 二、获取DOM对象
## 1.根据css选择器来获取DOM对象
### 选择匹配的第一个元素
document.querySelector('css选择器')，可以直接修改值

### 选择匹配的多个元素
`document.querySelectorAll('css选择器')`，不可以直接修改值，需要遍历得到每一个元素，得到一个有长度有索引号的伪数组，但是没有pop(),push()

## 2.其他获取DOM元素方法
根据id获取一个元素`document.getElementById('nav')`
根据标签获取一类元素获取页面所有div,`document.getElementsByTagName('div')`
根据类名获取元素获取页面所有类名为w的`document.getElementsByClassName('W')`

# 三、操作元素内容
## 1.对象.innerText 属性
将文本内容添加/更新到任意标签位置，显示纯文本，不解析标签

## 2.对象.innerHTML 属性
将文本内容添加/更新到任意标签位置，解析标签

# 年会抽奖案例
## 案例要求
一等奖二等奖三等奖随机各抽一个人，并且要求不重复

## 大致思路
### html:
放一个大盒子里有strong标签的加粗标题和三个不同量级的标题，标题里放span行内元素的盒子放人名

### css:
大致加些样式，也可以放背景图片

### js:渲染box
#### 1.获取随机数random
注意每一次抽奖后的数组长度都减一，所以设置一个变量 j 记录已经抽出的人数，如果不实时改变数组长度可能会抽出undefined

#### 2.获取DOM对象
用模板字符串实时改变获取的对象

#### 3.操作元素内容
将获取的随机人放入DOM对象中

#### 4.最后记得删除数组中抽出的那个随机对象

## 完整代码

```css
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        .box {
            width: 300px;
            height: 300px;
            margin:100px auto;
            background-color: #383ab5;
        }
    </style>
</head>
<body>
    <div class="box">
        <strong>年会抽奖</strong>
        <h1>一等奖:<span class="ww1"></span></h1>
        <h3>二等奖:<span class="ww2"></span></h3>
        <h5>三等奖:<span class="ww3"></span></h5>
    </div>
    <script>
        const arr=['a','b','c','d','e','f']
        let j=0
        for(let i=1;i<=3;i++){
            const random =Math.floor(Math.random()*(arr.length-j))
            const one = document.querySelector(`.ww${i}`)
            one.innerHTML = arr[random]
            arr.splice(random,1)
        }
    </script>
</body>
</html>
```