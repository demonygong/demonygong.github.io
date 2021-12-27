---
title: js事件循环
date: 2021-12-27 11:11:32
tags:
- 事件循环
categories:
- js
---
JS 单线程语言：指的是 javascript 引擎（如V8）在同一时刻只能处理一个任务；
事件循环唯一，可有多个队列任务。
<!--more-->
#### 任务队列( Event Queue )
> 所有的任务可以分为同步任务和异步任务，同步任务，顾名思义，就是立即执行的任务，同步任务一般会直接进入到主线程中执行；而异步任务，就是异步执行的任务，比如ajax网络请求，setTimeout 定时函数等都属于异步任务，异步任务会通过任务队列的机制(先进先出的机制)来进行协调。

#### 事件循环
**同步和异步任务分别进入不同的执行环境，同步的进入主线程，即主执行栈，异步的进入任务队列。主线程内的任务执行完毕为空，会去任务队列读取对应的任务，推入主线程执行。 上述过程的不断重复就是我们说的 Event Loop (事件循环)。**
![事件循环](/img/event-loop/loop1.png)

在事件循环中，每进行一次循环操作称为tick，每一次 tick 的任务处理模型是比较复杂的，其关键的步骤可以总结如下：
1. 执行一个宏任务（栈中没有就从事件队列中获取）
2. 执行过程中如果遇到微任务，就将它添加到微任务的任务队列中
3. 宏任务执行完毕后，立即执行当前微任务队列中的所有微任务（依次执行）
4. 当前宏任务执行完毕，开始检查渲染，然后GUI线程接管渲染/(render渲染)
5. 渲染完毕后，JS线程继续接管，开始下一个宏任务（从事件队列中获取）
![tick](/img/event-loop/loop2.png)

**宏任务主要包含：script( 整体代码)、setTimeout、setInterval、I/O、UI 交互事件、setImmediate(Node.js 环境)**
**微任务主要包含：Promise、MutaionObserver、process.nextTick(Node.js 环境)**

另外：
* new promise本身是同步的；
* await阻塞后面的代码执行，因此跳出async函数执行下一个微任务（<u>实际上await是一个让出线程的标志。await后面的表达式会先执行一遍，将await后面的代码加入到microtask中，然后就会跳出整个async函数来执行后面的代码</u>）

```
async function async1() {
    console.log('async1 start');
    await async2();
    console.log('async1 end');
}
async function async2() {
	console.log('async2');
}

console.log('script start');

setTimeout(function() {
    console.log('setTimeout');
}, 0)

async1();

new Promise(function(resolve) {
    console.log('promise1');
    resolve();
}).then(function() {
    console.log('promise2');
});
console.log('script end');


/*
script start
async1 start
async2
promise1
script end
async1 end
promise2
setTimeout
*/
```
