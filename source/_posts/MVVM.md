---
title: MVVM
date: 2022-10-11 10:04:45
categories:
- MVVM
tags:
- MVVM
---

![mvvm](/img/mvvm/mvc-mvvm.jpg)
<!--more-->
## MVC（单向通信）
M——model模型，获取数据，处理数据逻辑。
V——view视图，处理数据显示。
C——Controller控制器，从视图读取数据，控制用户输入，并向模型发送数据
>在MVC设计模式中， Model 响应用户请求并返回响应数据，View 负责格式化数据并把它们呈现给用户，业务逻辑和表示层分离，同一个 Model 可以被不同的 View 重用，所以大大提高了代码的可重用性。
MVC模式的三个模块相互独立，改变其中一个不会影响其他两个，所以依据这种设计思想能构造良好的少互扰性的构件。
Controller 提高了应用程序的灵活性和可配置性。

## MVVM
### 出现缘由
&ensp;&ensp;&ensp;&ensp;随着前端项目越来越大，项目的可维护性、可扩展性和安全性等成了主要问题，之前的浏览器兼容性问题已经退居其次。当年典型的类库如jquery，只能解决浏览器兼容性问题，但没有实现对业务逻辑的分成，所以维护性和扩展性较差，这才有了MVVM模式一类框架的出现
**Model 层:**
> 对应数据层的域模型，它主要做域模型的同步。通过 Ajax/fetch 等 API 完成客户端和服务端业务 Model 的同 步。在层间关系⾥，它主要⽤于抽象出 ViewModel 中视图的 Model 。

**View 层:**
> 作为视图模板存在，在 MVVM ⾥，整个 View 是⼀个动态模板。除了定义结构、布局外，它展示的是 ViewModel 层的数据和状态。 View 层不负责处理状态， View 层做的是数据绑定的声明、 指令的声明、 事件绑定的声明。

**ViewModel 层:**
> 把 View 需要的层数据暴露，并对 View 层的数据绑定声明、 指令声明、 事件绑定声明负责，也就是处理 View 层的具体业务逻辑。ViewModel 底层会做好绑定属性的监听。当 ViewModel 中数据变化， View 层会得到更新；⽽当 View 中声明了数据的双向绑定（通常是表单元素），框架也会监听 View 层（表单）值的变化。⼀旦值变化，View 层绑定的 ViewModel 中的数据也会得到⾃动更新。

<mark>M和V不能直接通信，只能通过VM（通过双向数据绑定把 View 层和 Model 层连接起来）。</mark>VM要实现一个observer观察者，VM监听到数据变化时，通知视图做自动更新；VM监听到用户操作的视图的变化，会通知数据做改动，从而实现数据的双向绑定。

### 优缺点
> **优点：**
>> * 分离视图（View）和模型（ Model ） , 降低代码耦合，提⾼视图或者逻辑的重⽤性。
>> * 提⾼可测试性 : ViewModel 的存在可以帮助开发者更好地编写测试代码。
>> * ⾃动更新 dom: 利⽤双向绑定 , 数据更新后视图⾃动更新 

> **缺点：**
>> * Bug 很难被调试 : 因为使⽤双向绑定的模式，当你看到界⾯异常了，有可能是你 View 的代码有 Bug ，也可能是 Model 的代码有问题。数据绑定使得⼀个位置的Bug 被快速传递到别的位置，要定位原始出问题的地⽅就变得不那么容易了。另外，数据绑定的声明是指令式地写在View 的模版当中的，这些内容是没办法去打断点 debug 的。
>> * ⼀个⼤的模块中 model 也会很⼤，虽然使⽤⽅便了也很容易保证了数据的⼀致性，当时⻓期持有，不释放内存就造成了花费更多的内存。
>> * 对于⼤型的图形应⽤程序，视图状态较多， ViewModel 的构建和维护的成本都会⽐较⾼。

### 双向绑定的核心： Object.defineProperty()
Object.defineProperty(obj, prop, descriptor) 方法会直接在一个对象上定义一个新属性，或者修改一个对象的现有属性，并返回此对象。
> obj：要定义属性的对象
> prop：要定义或修改的属性的名称或 Symbol
> descriptor：要定义或修改的属性描述符
> 返回值：被传递给函数的对象

通过Object.defineProperty的get方法用来获取值 set方法用来拦截设置值
```
var obj = {};  //定义一个空对象
Object.defineProperty(obj, 'val', {//定义要修改对象的属性
	get: function () {
		console.log('获取对象的值')
	},
	set: function (newVal) { 
		console.log('设置对象的值：最新的值是'+newVal);
	}
});
obj.hello = 'hello'
```
js通过Object.defineProperty方法简单的实现双向绑定
```
<!DOCTYPE html>
<html lang="en">
	<head>
		<meta charset="UTF-8">
		<meta http-equiv="X-UA-Compatible" content="IE=edge">
		<meta name="viewport" content="width=device-width, initial-scale=1.0">
		<title>Document</title>
	</head>
	<body>
		<input type="text" id="app">
		<span id="childSpan"></span>
	</body>
	<script>
		var obj = {}
		var initValue='初始值'
		Object.defineProperty(obj,'initValue',{
			get(){
				console.log('获取obj最新的值');
				return initValue
			},
			set(newVal){
				initValue = newVal
				console.log('设置最新的值');
				// 获取到最新的值  然后将最新的值赋值给我们的span
				document.getElementById('childSpan').innerHTML = initValue
				console.log(obj.initValue);
			}
		})
		document.addEventListener('keyup', function (e) {
			obj.initValue = e.target.value; //监听文本框里面的值 获取最新的值 然后赋值给obj 
		})
	</script>
</html>
```

### 双向绑定过程
**1、初步了解四个东西:**
* 0bserver监听器——监听data选项中属性是被访问或被改变，决定调用getter/setter。
* Compile指令解析器——解析元素节点的指令，初始化视图，订阅watcher来更新视图。
* Watcher订阅者——是Observer和Compile的桥梁，订阅并收到属性变动通知，执行指令绑定的相应回调函数。
* Dep消息订阅器——它的内部有一个用来收集订阅者的数组，数据变动触发notify函数，再调用订阅者的update方法。

**2、如何实现双向绑定**
（1）初始化阶段:
- 监听：Observer把js对象传给vue实例的data选项，vue遍历data选项中的属性，并用Object.defineProperty()方法将这些属性转成setter/getter方法，实现监听功能;
- Compile指令编译器，解析元素节点的指令，初始化视图，并订阅Watcher来更新视图;
- watcher将自己添加到消息订阅器Dep，初始化完毕。

（2）数据变化时:
- Observer中的setter方法被触发, setter调用Dep.notify();
- Dep开始遍历所有订阅者，并调用订阅者的update方法;
- 订阅者收到通知后，更新视图。

### 双向绑定过程图解
![mvvm](/img/mvvm/mvvm.jpg)

#### 任务拆分：
* 将vue实例中的数据渲染到页面上
* 将页面上的数据变更同步到vue实例中
* vue实例中data数据变更，页面上数据同步变更