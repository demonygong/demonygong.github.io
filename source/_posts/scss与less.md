---
title: scss与less
date: 2021-04-29 15:18:09
tags:
- scss
categories:
- css
---

scss和less都属于css预处理器（css编程语言，编译后为正常css文件供项目使用）。相比较个人更倾向于scss,所以着重记录下两者主要区别：
<!--more-->
#### 为什么使用css预处理器
##### css缺点
- css仅仅是标记语言，而非编程语言。不能像js那样定义变量、可引用等
- 语法不够强大，无法嵌套书写，导致模块化会有很多重复代码
- 没有变量和合理的样式复用机制，重复过多之后，难以维护

#### sass和less的比较：
##### 不同之处
1. less环境较scss简单
 - scss基于Ruby环境在服务器端处理，也就是需要安装Ruby环境（面向对象脚本语言，别的不知道，，）。
 - less基于js在浏览器端处理，也就是在页面中引入less.js便可直接是使用。
2. less使用较scss简单
 - less没有裁剪css原有特性，实在现有基础上加入一些编程语言的特性。所以只要会css，less很容易上手。
3. 从功能出发，sass比less略强大一些（具体就自行比较吧）
 - [scss中文网](https://www.sass.hk/docs/)
 - [less中文网](http://lesscss.cn/)
4. less与sass处理机制不一样
 - 前者是通过客户端处理的，后者是通过服务端处理，相比较之下前者解析会比后者慢一点

##### 相同之处
1. 混入(Mixins)——class中的class；
2. 参数混入——可以传递参数的class，就像函数一样；
3. 嵌套规则——Class中嵌套class，从而减少重复的代码；
4. 运算——CSS中用上数学；
5. 颜色功能——可以编辑颜色；
6. 名字空间(namespace)——分组样式，从而可以被调用；
7. 作用域——局部修改样式；
8. JavaScript 赋值——在CSS中使用JavaScript表达式赋值。

#### 为什么选择使用scss
1. Sass在市面上有一些成熟的框架，比如说Compass，而且有很多框架也在使用Sass，比如说Foundation。
2. 就国外讨论的热度来说，Sass绝对优于LESS。
3. 就学习教程来说，Sass的教程要优于LESS。在国内LESS集中的教程是LESS中文官网，而Sass的中文教程，慢慢在国内也较为普遍。
4. Sass也是成熟的CSS预处理器之一，而且有一个稳定，强大的团队在维护。
5. 同时还有Scss对sass语法进行了改良，Sass 3就变成了Scss(sassy css)。与原来的语法兼容，只是用{}取代了原来的缩进。
6. bootstrap（Web框架）最新推出的版本4，使用的就是Sass。
