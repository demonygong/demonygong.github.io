---
title: 页面缓存
date: 2019-11-15 17:45:39
tags: cookie
---
前端页面常用缓存，具体设置哪种根据项目需求而定。分为三种：
* localstorage缓存
* cookie缓存：
* sesionstorage缓存
<!--more-->

## localstorage缓存:
&ensp;&ensp;&ensp;&ensp;localStorage用于持久化的本地存储，除非主动删除数据，否则数据是永远不会过期的。同源可以读取并修改localStorage数据。
```
// 缓存数据字典
      api.dictionary_get({type:'Gender'}).then(res=>{
          localStorage.setItem("Gender", JSON.stringify(res.data));
      })
 	localStorage.setItem("Gender", “data”);

 	localStorage.getItem("Gender");
```

## cookie缓存：
&ensp;&ensp;&ensp;&ensp;默认时效会话级别，即不设置存活时间 是保存在浏览器内存中，关闭浏览器则内存消失；如果设置保存时间，将保存在浏览器硬盘中，如果不超时就一直存在，超时则消失。
```
    setCookie(name,value){
        var exp = new Date();
        exp.setTime(exp.getTime() + 2*30*24*60*60*1000);
        document.cookie = name + "="+ escape (value) + ";expires=" + exp.toGMTString();
    },

    getCookie(name){
        var arr,reg=new RegExp("(^| )"+name+"=([^;]*)(;|$)");
        if(arr=document.cookie.match(reg))
            return unescape(arr[2]);
        else
            return null;
    },
```
## sessionstorage缓存:
&ensp;&ensp;&ensp;&ensp;sessionStorage用于本地存储一个会话（session）中的数据，这些数据只有在同一个会话中的页面才能访问,并且当会话结束后数据也随之销毁。因此sessionStorage不是一种持久化的本地存储，仅是会话级别的存储。只允许同一窗口访问。用法同localStorage。

## 三者异同：
![funnel](/img/storage.png)
