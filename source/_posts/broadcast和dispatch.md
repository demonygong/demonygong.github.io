---
title: vue之派发与广播-dispatch与broadcast
date: 2021-10-18 16:58:09
categories:
- Vue
tags:
- broadcast
- dispatch
---

#### 主要针对组件之间的跨级通信
<!--more-->
#### 为什么不使用bus?
eventBus，方便易用，可在你想组合的组件之间随意使用，甚至没有级别关系的组件，只要在一个组件系统中，就可以通信。但是，如果你想完成一个通用组件，而且它有专用的子级组件。当页面多次使用这个组件时，eventBus在触发时就会发生混乱。这时，就需要一个指代父子关系的约束。**（主要用于兄弟组件通信）**


#### 为什么不使用provide与inject?
因为它的使用场景，主要是子组件获取上级组件的状态，跨级组件间建立了一种主动提供与依赖注入的关系。
然后有两种场景它不能很好的解决：
- 父组件向子组件（跨级）传递数据；
- 子组件向父组件（跨级）传递数据。


#### emitter.js
```
// broadcast(广播)，向下冒泡
// dispatch(派发)，向上冒泡


function broadcast(componentName, eventName, params) {
  this.$children.forEach(child => {
    const name = child.$options.name;

    if (name === componentName) {
      child.$emit.apply(child, [eventName].concat(params));
    } else {
      // todo 如果 params 是空数组，接收到的会是 undefined
      broadcast.apply(child, [componentName, eventName].concat([params]));
    }
  });
}
export default {
  methods: {
    dispatch(componentName, eventName, params) {
      let parent = this.$parent || this.$root;
      let name = parent.$options.name;

      while (parent && (!name || name !== componentName)) {
        parent = parent.$parent;

        if (parent) {
          name = parent.$options.name;
        }
      }
      if (parent) {
        parent.$emit.apply(parent, [eventName].concat(params));
      }
    },
    
    broadcast(componentName, eventName, params) {
      broadcast.call(this, componentName, eventName, params);
    }
  }
};
```

#### 父子组件
```
parent.vue

<template>
  <div>
    <div @click="emitClick">emitter</div>
	<chlidone></chlidone>
  </div>
</template>
<script>
import emitterMixin from "@/mixins/emitterMixin.js";
import chlidone from './chlidone.vue'
export default {
  name: "parent",
  mixins: [emitterMixin],
  components: { chlidone },
  created() {
    this.$on("toParent", this.handleChild);
  },
  methods: {
    handleChild(val) {
      alert(val);
    },
    emitClick(){
        this.broadcast("chlidone", "toChild", "向下冒泡");
    },
  },
};
</script>

// child
<template>
  <div>
    <div @click="childClick">23333</div>
  </div>
</template>

<script>
import emitterMixin from "@/mixins/emitterMixin.js";
export default {
  name: "chlidone",
  mixins: [emitterMixin],
  data() {
    return{}
  },
  created() {
    this.$on("toChild", this.handleChild);
  },
  methods:{
    childClick(){
      this.dispatch("parent", "toParent", "向上冒泡");
    },
    handleChild(val) {
      alert(val);
    }
  }
}
</script>
```
这样就能实现跨级组件自定义通信了，但是，要注意其中一个问题：订阅必须先于发布，也就是说先有on再有emit。

#### 父子组件渲染顺序，实例创建顺序
子组件先于父组件前渲染，所以在子组的mounted派发事件时，在父组件中的mounte中是监听不到的。
而父组件的create是先于子组件的，所以可以在父组件中的create可以监听到

