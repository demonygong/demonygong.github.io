---
title: 'element表格倒计时'
date: 2021-04-27 15:17:09
categories:
- Vue
- element
tags:
- 倒计时
---

element动态表格数据倒计时处理。。。
<!--more-->

#### 表格渲染倒计时：
```
// nowtime 获取进入页面的当前时间戳
// 放入computed中！！
{{scope.row.time|countdown(nowtime)}}
```

#### filters过滤：
```
filters:{
    countdown(end, nowtime) {
      var mistiming = end - nowtime //   获取截止时间距离现在的毫秒差
      if(mistiming>0){
        var days = Math.floor(mistiming / 1000 / 60 / 60 / 24); //获取天数
        var hours = Math.floor(mistiming / 1000 / 60 / 60 % 24); // 获取小时
        var minutes = Math.floor(mistiming / 1000 / 60 % 60); //获取分钟数
        var seconds = Math.floor(mistiming / 1000 % 60)+1; //获取秒数
        days < 10 ? days = "0" + days : days;
        hours < 10 ? hours = "0" + hours : hours;
        minutes < 10 ? minutes = "0" + minutes : minutes;
        seconds < 10 ? seconds = "0" + seconds : seconds;
        var rels = `${days}天${hours}时${minutes}分${seconds}秒`
      }
      // 判断是否到截止时间
      var mis = mistiming > 0 ? rels : "已截止"
      return mis
    }
},
```

#### created表格加入定时器：
```
this.tableData.forEach(el=>{
    this.daojishi(el);//调用定时器
})
```

#### 倒计时定时器：
```
daojishi(row) {
    row.countDown = setInterval(() => {
    if(row.time>0){
        row.time = row.time - 1000;
    }else{
        clearInterval(row.countDown)
    }
    }, 1000);
},
```

最后就是离开页面销毁前清除定时器即可。


