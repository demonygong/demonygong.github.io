---
title: 'VDOM'
date: 2022-04-07 10:13:18
categories:
- Vue
- VDOM
tags:
- VDOM
---

![vdom](/img/vdom/vdom2.png)
<!--more-->

### 什么是DOM？
Document Object Model(文档对象模型)，是为HTML和XML提供的API；
按照DOM的标准，HTML和XML都是以标签为结点构造的树结构，DOM将HTML和XML的相同的结构本质抽象出来，然后通过脚本语言，如Javascript，按照DOM里的模型标准访问和操作文档内容。

### 什么是VDOM？
虚拟DOM： virtual DOM ，用普通js对象来描述DOM结构，因为不是真实DOM，所以称之为虚拟DOM。

### VDOM和真实DOM的区别？
VDOM是将真实的DOM的数据抽取出来，以对象的形式模拟树形结构:
真实DOM:
```
    <div>
        <p>123</p>
    </div>
```
对应的virtual DOM（伪代码）：
```
    var Vnode = {
        tag: 'div',
        children: [
            { tag: 'p', text: '123' }
        ]
    };
```

### 虚拟DOM要解决的问题 ：
- 在react，vue等技术出现之前：
要改变页面展示的内容，只能通过遍历查询 dom 树的方式找到需要修改的 dom ，
然后修改样式行为或者结构，来达到更新 ui 的目的
相当消耗计算资源（每次查询 dom 几乎都需要遍历整颗 dom 树）
- 使用虚拟DOM（ js 对象）：每次 dom 的更改就变成了 js 对象的属性的更改，这样一来就能查找 js 对象的属性变化，要比查询 dom 树的性能开销小。
虚拟DOM就是为了 **解决浏览器性能问题** 而被设计出来的。

### 页面渲染过程：
DOM树=》CSS树=》Render树
![vdom](/img/vdom/vdom3.png)

### 为什么操作 dom 性能开销大？
并不是查询 dom 树性能开销大，
原因：
- dom树的实现模块 和 js 模块 是分开的，这些跨模块的通讯增加了成本。
- dom 操作引起的浏览器的回流和重绘，使得性能开销巨大。

### # 举个例子：
在一次操作中，需要更新10个DOM节点，浏览器收到第一个DOM请求后并不知道还有9次更新操作，因此会马上执行流程，最终执行10次。第一次计算完，紧接着下一个DOM更新请求，这个节点的坐标值就变了，前一次计算为无用功。计算DOM节点坐标值等都是白白浪费的性能。
**而虚拟DOM不会立即操作DOM，而是将这10次更新的diff内容保存到本地一个JS对象中，最终将这个JS对象一次性附到DOM树上，再进行后续操作，避免大量无谓的计算量**

### diff算法：
这里修改某个数据，如果直接渲染到真实dom上会引起整个dom树的重绘和重排，通过diff算法定位出需要修改（值变化）的地方
在采取diff算法比较新旧节点的时候，比较只会在同层级进行, 不会跨层级比较。
![vdom](/img/vdom/vdom4.png)