---
title: js操作属性和定时器以及相关案例
date: 2024-07-22 15:51:58
updated: 2024-07-23 13:14:09
categories: js
tags: [js]
---
# 一、操作元素属性


## 1.操作元素常用属性


对象.属性=值
## 2.操作元素样式属性


### 通过style属性操作css


对象.style.样式属性=值
//获取元素
const box document.querySelector('.box')
//修改元素样式
#### 1.修改样式通过style属性引出


box.style.width '200px
box.style.marginTop ='15px
#### 2.如果属性有-连接符，需要转换为小驼峰命名法


box.style.backgroundColor ='pink'
#### 3.赋值的时候，需要的时候不要忘记加css单位


### 通过className操作css


元素.className='类名(不加点)'
**注意：**
由于class是关键字，所以使用className去代替
className是使用新值换旧值实现覆盖，如果需要添加一个类，需要保留之前的类名

### 通过classList操作css


使用className容易覆盖以前的类名，可以使用classList方式追加和删除类名
//追加一个类
元素.classList.add('类名')**类名不加点且是字符串**
//删除一个类
元素.classList.remove('类名')
//切换一个类(有就删，没有就加)
元素.classList.toggle('类名')

## 3.操作表单元素属性


获取：DOM对象.属性名
设置：DOM对象.属性名=新值
表单.value='用户名'
表单.type='password'
**注意**：获取表单内容只能用"表单.value",但button特殊使用button.innerHTML,因为button的已经是标签内的内容，innerHTML获取的是双标签内的提示内容

表单属性中添加就有效果，移除就没有效果，一律使用布尔值表示如果为true代表添加了该属性如果是false代表移除了该属性
比如：disabled禁用、checked勾选、selected

```javascript
<button disabled></button>
const button=document.querySeletor('button')
button.disabled=false//表示不能禁用
```

## 4.自定义属性


自定义属性：
在html5中推出来了专门的data-自定义属性
在标签上一律以data-开头
在DOM对象上一律以dataset对象方式获取

```css
<body>
    <div class="box",data-id="10",data-smp="21">盒子</div>
<script>
    const box document.querySelector('.box')
    console.log(box.dataset.id)
    console.log(box.dataset.smp)
</script>
</body>
```

# 五、定时器-间歇函数


单位为毫秒，返回的是一个id数字，函数名不需要加括号
## 打开定时器


1. 
```javascript
setInterval (function(){
    console.log('一秒执行一次')
},1000)
//1000毫秒即为1秒，数字越小表示跳转间隔越小，跳转越快
```

2. 
```javascript
function fn() {
    console.log('一秒执行一次')
}
setInterval(fn,1000)
//这里调用函数不要加(),fn()表示立即调用函数，但在定时器里是主动调用
```
## 关闭定时器


1. 
```javascript
let timer = setInterval(function(){
console.log('hi~~')
},1000)
clearInterval(timer)
```

2. 
```javascript
let 变量名 = seyInterval(函数，间隔函数)
clearInterval(变量名)
```

# 用户倒计时效果案例


## 案例要求


文本框里放用户协议，倒计时结束前不能点击同意
## 大致思路


设置一个含有倒计时的按钮，获取按钮对象，利用定时器函数，不断修改倒计时内容，当倒计时数字为0时，关闭定时器，将禁用取消，文字改为同意


## 完整代码

```css
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <textarea name="" id="" col="30" rows="10">
        用户注册协议
        xxx是哈哈怪，看完才能点确认
    </textarea>
    <br>
    <button class="btn" disabled>确认(8)</button>
    <script>
        let i=8
        const btn=document.querySelector('.btn')
        let n=setInterval(function(){
            i--
            btn.innerHTML=`确认${i}`
            if(i===0){
                clearInterval(n)
                btn.disabled=false
                btn.innerHTML='同意'
            }
        },1000)
    </script>
</body>
</html>
```
