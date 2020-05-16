---
layout: computed
title: computed() VS methods()
date: 2019-08-06 17:29:17
categories:
- Vue
---

&ensp;&ensp;&ensp;&ensp;可以使用 methods 来替代 computed(计算机属性)，效果上两个都是一样的，但是 computed 是基于它的依赖缓存，只有相关依赖发生改变时才会重新取值。而使用 methods ，在重新渲染的时候，函数总会重新调用执行。
<!--more-->

Date.now() 不是响应式依赖，这样的computed不会再次更新：
```
computed: {
  now: function () {
    return Date.now()
  }
}
```
---
响应式依赖：
```
computed: {
  reversedMessage: function () {
    return this.message.split('').reverse().join('')
    //这里的message是同一个组件里面的data()里边定义好了的一个变量
  }
}
 ```
&ensp;&ensp;&ensp;&ensp;message每次变动的时候，都会执行一次computed，把它刚获得的新值再“倒置”。
&ensp;&ensp;&ensp;&ensp;相比而言，只要发生重新渲染，method 调用总会执行该函数。

&ensp;&ensp;&ensp;&ensp;没有使用到计算属性的依赖缓存的时候，可以使用定义方法来代替计算属性，在 methods 里定义一个方法可以实现相同的效果，甚至该方法还可以接受参数，使用起来更灵活。

__computed必须返回一个值页面绑定的才能取得值，而methods中可以只执行逻辑代码，返回值可有可无。__