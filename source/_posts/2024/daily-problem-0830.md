---
title: js之原型
date: 2024-09-07 19:51:58
updated: 2024-09-08 10:14:09
categories: js
tags: [js]
---
# 一、编程思想
## 1.面向过程编程


面向过程就是分析出解决问题所需要的步骤，然后用函数把这些步骤一步一步实现，使用的时候再一个一个的依次
调用就可以了
优点：性能好
缺点：不灵活，复用性低


## 2.面向对象编程


面向对象是把事务分解成为一个个对象：然后由对象之间分工与合作。
1. 每一个对象都是功能中心，具有明确分工
2. 面向对象编程具有灵活、代码可复用、容易维护和开发的优点，更适合多人合作的大型软件项目。
3. 面向对象的特性：封装性,继承性,多态性


# 二、构造函数


封装是面向对象思想中比较重要的一部分，jS面向对象可以通过构造函数实现的封装。
同样的将变量和函数组合到了一起并能通过thS实现数据的共享，所不同的是借助构造函数创建出来的实例对象之
间是彼此不影响的


**总结：**
1. 构造函数体现了面向对象的封装特性
2. 构造函数实例创建的对象彼此独立、互不影响
3. 但是可能存在浪费内存的问题


# 三、原型


## 1. 原型


JavaScript规定，每一个构造函数都有一个prototype属性，指向另一个对象，所以我们也称为原型对象
对象实例化不会多次创建原型上函数，节约内存
1. 我们可以把那些不变的方法，直接定义在prototype对象上，这样所有对象的实例就可以共享这些方法。
2. 构造函数和原型对象中的this都指向实例化的对象
const arr=[1,2,3]
Array.prototype.max=function(){
    return Math.max(...this)
    //原型函数里面的this指向谁？实例对象
}
console.log(arr.max())


## 2.constructor属性


每个原型对象里面都有个constructor属性
**作用：**
该属性指向该原型对象的构造函数，简单理解，就是指向我的爸爸，我是有爸爸的孩子
**使用场景：**
如果有多个对象的方法，我们可以给原型对象采取对象形式赋值
但是这样就会覆盖构造函数原型对象原来的内容，这样修改后的原型对象constructor就不再指向当前构造函数了
此时，我们可以在修改后的原型对象中，添加一个constructor指向原来的构造函数。


## 3.对象原型


对象都会有一个属性_proto指向构造函数的prototype原型对象，之所以我们对象可以使用构造函数prototype
原型对象的属性和方法，就是因为对象有_proto_原型的存在。


**注意：**
_proto_是JS非标准属性
[[prototype]]和_proto_意义相同
用来表明当前实例对象指向哪个原型对象prototype
_proto_对象原型里面也有一个constructor属性，指向创建该实例对象的构造函数 


## 4.原型继承


继承是面向对象编程的另一个特征，通过继承进一步提升代码封装的程度，JavaScript中大多是借助原型对象实现继承
的特性


**核心：**抽取公共部分为构造函数，子类的原型继承new父类


```javascript
function Person(){
    this.eyes=2
    this.head=1
}
function Woman(){
}
//Woman通过原型来继承Person
Woman.prototype=new Person()
//指回原来的构造函数
Woman.prototype.constructor=Woman!

```

## 5.原型链


基于原型对象的继承使得不同构造函数的原型对象关联在一起，并且这种关联的关系是一种链状的结构，我们将原型对
象的链状结构关系称为原型链


**查找规则**
1. 当访问一个对象的属性（包括方法）时，首先查找这个对象自身有没有该属性。
2. 如果没有就查找它的原型(也就是proto指向的prototype原型对象)
3. 如果还没有就查找原型对象的原型（Object的原型对象)
4. 依此类推一直找到Object为止(null)
5. _proto_对象原型的意义就在于为对象成员查找机制提供一个方向，或者说一条路线
6. 可以使用instanceof运算符用于检测构造函数的prototype属性是否出现在某个实例对象的原型链上
Array instanceof Object(true)数组属于对象