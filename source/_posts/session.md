---
title: 会话
date: 2020-05-16 10:04:25
tags: session
---

&ensp;&ensp;&ensp;&ensp;session这个词经常跟会话绑定在一块，一次session就是一次会话。所谓的一次会话就是浏览器和服务器的一次通话。从打开浏览器访问一个服务器开始，就是创建了一个session，会话就开始了。
<!--more-->

![funnel](/img/session/session.png)

&ensp;&ensp;&ensp;&ensp;服务器创建session、cookie，服务器保存session信息，但有一个唯一标识符sessionid保存在cookie中发送给浏览器，再访问时根据sessionid找session信息。
&ensp;&ensp;&ensp;&ensp;cookie：不设置存活时间 是保存在浏览器内存中，关闭浏览器则内存消失；如果设置保存时间，将保存在浏览器硬盘中，如果不超时就一直存在，超时则消失。
* session销毁方式：
    + session默认存活时间只有三十分钟，超时即销毁；
    + 服务器非正常关闭，session即消失；

&ensp;&ensp;&ensp;&ensp;同时在同一个浏览器上先后访问同一个服务器的session的id是相同的，而不同浏览器上访问同一个服务器的session的id是不同的。