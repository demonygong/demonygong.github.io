---
title: 跨页面跳转切换tab
date: 2020-05-28 18:26:40
tags:
- tab选项卡
categories:
- js
---

&ensp;&ensp;&ensp;&ensp;**JavaScript跳转到指定页面并且到指定页面的tab**。案例的解析就是点击A页面里的某个地方或者按钮，跳转到B页面的相应tab选项卡，显示相应B页面内容。
&ensp;&ensp;&ensp;&ensp;思路：从页面 A 通过 url后面的参数给页面B 传一个 值，页面b通过这个参数来控制该选项卡的切换。
<!--more-->
```
a.html
<div class="footer">
           <span><a href="{:url('index/about/index')}?1">关于我们</a> </span>
           <span><a href="{:url('index/about/index')}?2">团队介绍</a></span>
           <span><a href="{:url('index/about/index')}?3">联系我们</a></span>
        </div>

b.html
 <div class="tab_menu">
            <ul>
                <li class="selected">公司介绍</li>
                <li>团队介绍</li>
                <li>企业文化</li>
                <li>经营理念</li>
                <li>联系我们</li>
            </ul>
        </div>
        <div class="tab_box">
            <div class="tab_con"><div>公司</div> </div>
            <div class="tab_con"><div>团队介绍</div> </div>
            <div class="tab_con"><div>企业文化</div> </div>
            <div class="tab_con"><div>经营理念</div> </div>
            <div class="tab_con"><div>联系我们</div> </div>
        </div>

b.html  js部分
var type = location.href.split('?')[1]
    function tab_change() {
        var li_a= $(".tab_menu ul li");
        li_a.click(function(){
            $(this).addClass("selected");
            $(this).siblings().removeClass("selected");
            var index =  li_a.index(this);
            $(".tab_box > div").eq(index).show().siblings().hide();
        });
    }
    if(!type){
        $(".tab_box > div").eq(0).show().siblings().hide();
        tab_change()
    }else{
        if(type=='1'){
            $('.tab_menu li').eq(0).addClass("selected").siblings().removeClass("selected");
            $(".tab_box .tab_con").eq(0).show().siblings().hide();
            tab_change()
        }else if(type=='2'){
            $('.tab_menu li').eq(1).addClass("selected").siblings().removeClass("selected");
            $(".tab_box .tab_con").eq(1).show().siblings().hide();
            tab_change()
        }else if(type=='3'){
            $('.tab_menu li').eq(4).addClass("selected").siblings().removeClass("selected");
            $(".tab_box .tab_con").eq(4).show().siblings().hide();
            tab_change()
        }
    }
```