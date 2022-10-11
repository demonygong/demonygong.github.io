---
title: echarts点击切换路由
date: 2019-08-12 18:17:07
categories:
- Vue
tags:
- echarts
---

&ensp;&ensp;&ensp;&ensp;vue中使用echarts，然后点击echars图表进行切换路由（以funnel为例）：
![funnel](/img/funnel/funnel1.png)
<!--more-->

&ensp;&ensp;&ensp;&ensp;跳转界面因所需对dom节点操作，所以使用了mounted；
&ensp;&ensp;&ensp;&ensp;为过滤器赋值（路由跳转传入），如果直接在方法中赋值会比较死板，过滤器一直绑定传入数据：
![funnel](/img/funnel/funnel2.png)

&ensp;&ensp;&ensp;&ensp;使用activated（进入当前存在activated()函数的页面时，一进入页面就触发）：
![funnel](/img/funnel/funnel3.png)

&ensp;&ensp;&ensp;&ensp;但是重新刷新此页面时会发现，mounted和catived都会执行；因此，加上判断即可。
![funnel](/img/funnel/funnel4.png)