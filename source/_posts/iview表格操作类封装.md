---
title: iview表格操作类封装
date: 2020-06-15 17:42:25
categories:
- Vue
tags:
- iview
---


&ensp;&ensp;&ensp;&ensp;最近接触的iview的项目较多，iview使用起来也较容易上手。只是有一点 ，表格中部分需要操作更改的显示只能放在render中修改，少数还好，一旦多了会显得columns里代码很繁琐。
<!--more-->
&ensp;&ensp;&ensp;&ensp;一旦复杂程度多一点，data中就会显示很多代码，而实际在data返回中一般是不需要逻辑操作的（个人认为），所以视情况封装。
&ensp;&ensp;&ensp;&ensp;以下是针对操作按钮的，比如需要在table中显示编辑、删除等。

#### table显示
![funnel](/img/iview_table/iview_table.jpg)

#### 初始加载
就按钮而言，可定义多个函数（可传递多个参数，实际根据自己所需）
![funnel](/img/iview_table/iview_table2.jpg)
#### 点击按钮
点击按钮时传递相关参数，打开所需要编辑的modal。
![funnel](/img/iview_table/iview_table3.jpg)
回过头来看，此时的columns中我们只需要这么写，在data中就不会有过多的复杂代码：
![funnel](/img/iview_table/iview_table4.jpg)



