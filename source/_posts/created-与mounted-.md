---
title: created()与mounted()
date: 2019-08-05 18:55:22
categories:
- Vue
tags:
- vue钩子
---

&ensp;&ensp;&ensp;&ensp;created()：在创建vue对象时，当html渲染之前就触发；但是注意，全局vue.js不强制刷新或者重启时只创建一次，也就是说，created()只会触发一次；created 这个钩子在实例被创建之后被调用。一般可以在created函数中调用ajax获取页面初始化所需的数据。
<!--more-->
示例：
```
<template>
	<div class="parent">
		<div id="name">{{ name }}</div>
	</div>
</template>
<script>
	export default{
		data(){
			return{
				name: hello
			}
		},
		created: function(){
			console. log( document. getElementById("name").innerHTML);
		}
	}
<script>
```
出现如下报错：
```
TypeError: Cannot read property 'innerHTML Of null'
at Vue Component created (parent, vue?cad: 17)
at callHook (vue, esm. is2efeb: 2921)
at VueComponent. Vue, init (yue, esm. is2efeb: 4630)
at new Vue Component (yue, esm, is?feb: 4798)
at createComponent InstanceForVnode (yue, esm. is2efeb: 4310)
at init (yue, esm, is?feb: 4131)
at createComponent (yue, esm, is?feb: 5608)
at createElm (yue, esm.is2efeb: 5555)
at createChildren (vue esm is2efeb 5682)
```
挂载阶段还没开始，也就是说，模板还没有被渲染成html；也就是这时候通过id什么的去查找页面元素是找不到的

mounted()：mounted钩子函数一般是用来向后端发起请求拿到数据以后做一些业务处理，该钩子函数是在挂载完成以后也就是模板渲染完成以后才会被调用（vue的生命周期中一个实例的mounted只会运行一次）
示例：
```
<template>
	<div class="parent">
		<div id="name">{{ name }}</div>
	</div>
</template>
<script>
	export default{
		data(){
			return{
				name: hello
			}
		},
		mounted: function(){
			console. log( document. getElementById("name").innerHTML);
		}
	}
<script>
```
结果是：hello
取到了值，这说明这时候vue模板已经渲染完毕。因此，Dom操作一般是在mounted钩子函数中进行的

&ensp;&ensp;&ensp;&ensp;通常created使用的次数多，而mounted通常是在一些插件的使用或者组件的使用中进行操作，比如插件chart.js的使用: var ctx = document.getElementById(ID);通常会有这一步，而如果你写入组件中，你会发现在created中无法对chart进行一些初始化配置，一定要等这个html渲染完后才可以进行，那么mounted就是不二之选。
&ensp;&ensp;&ensp;&ensp;methods:{}中的方法都需要主动去触发，比如点击click之类的
而created(){}、mounted(){}、里面的代码都是自动去执行的，即vue生命周期到了哪一步就直接去执行对应钩子函数里面的代码了，无需手动去执行
&ensp;&ensp;&ensp;&ensp;created中主要放初始化获取数据之类，mounted()中挂载到具体的DOM节点