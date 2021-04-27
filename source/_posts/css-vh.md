---
title: css_vh
date: 2019-07-31 15:30:02
categories:
- css
tags:
- css
---

css中height 100vh的应用场景，动态高度百分比布局，浏览器视区大小单位

    height:100vh

<!--more-->
#### 一些只能vw, vh才能完成的应用场景：

##### 场景之：元素的尺寸限制

vw vh 主要是实现了动态高度百分比布局，比如宽高比不固定的图片，vw很轻易的实现正方形图片缩略图
原始大图的尺寸限制问题——因为很有可能图片过大，尼玛一屏显示器区域不够放，我们需要对其进行缩放处理。
这类限制的实现，在当下，需要获得图片的原始大小，以及浏览器内部尺寸，算大小，算比例等，算是比较折腾的。
但是，vw, vh等单位本身就是浏览器视区大小相关单位，直接使用其做限制，岂不省了N多JS代码？
CSS代码：
.vw_vh_img {
max-width: 90vw;
max-height: 90%;
max-height: 90vh;
}
##### CSS3新vw, vh单位与纯CSS定位的弹框屏幕居中效果实例页面
##### 视区覆盖以及边界定位

vh确实是相对于屏幕的，但默认body有一个margin，100%加上这个margin就超出了就会出现滚动条。清除body的margin即可。
body{margin:0;}


#### 在做手机端的时候经常会用到的做字体的尺寸单位
##### em
说白了 em就相当于“倍”，比如设置当前的div的字体大小为1.5em，则当前的div的字体大小为：当前div继承的字体大小*1.5
但是当div进行嵌套的时候，em始终是按照当前div继承的字体大小来缩放，参照后面的例子。
##### rem
这里的r就是root的意思，意思是相对于根节点来进行缩放，当有嵌套关系的时候，嵌套关系的元素的字体大小始终按照根节点的字体大小进行缩放。
参照后面给的demo
##### vh
vh就是当前屏幕可见高度的1%，也就是说
height:100vh == height:100%;
但是有个好处是当元素没有内容时候，设置height:100%该元素不会被撑开，
但是设置height:100vh，该元素会被撑开屏幕高度一致。
##### vw
vw就是当前屏幕宽度的1%
补充一句，当设置width:100%，被设置元素的宽度是按照父元素的宽度来设置，
但是100vw是相对于屏幕可见宽度来设置的，所以会出现50vw 比50%大的情况


#### 代码示例
```
<!DOCTYPE html>
<html lang="Zh-cn">
<head>
<meta charset="UTF-8">
<title>一程烟雨</title>
</head>
<style type="text/css" media="screen">
html{
font-size: 14px;
}
.em,
.em > .em-son,
.em > .em-son > .em-grandson {
font-size: 1.2em;
}
.rem,
.rem > .rem-son,
.rem > .rem-son > .rem-grandson {
font-size: 1.2rem;
}
.rem-box {
background: #d60b3b;
width:10rem;
height: 10rem;
color: #fff;
text-align: center;
line-height:5rem;
}
.vhvw-box {
background: #d60b3b;
width:50vw;
height: 50vh;
color: #fff;
text-align: center;
line-height:25vh;
}
</style>
<body>
<h1>em 继承父元素的字体大小，来变大或变小，多层嵌套字体变化</h1>
<div class="em">
字体大小 1.2 * 14（父元素body） = 16px
<div class="em-son">
字体大小 1.2 * 16(父元素em) = 20px
<div class="em-grandson">
字体大小 1.2 * 20(父元素em-son) = 24px
</div>
</div>
</div>
<br>
<h1>rem 继承根节点元素的字体大小，来变大或变小，多层嵌套字体不变化</h1>
<div class="rem">
字体大小 1.2 * 14（根节点html） = 16px
<div class="rem-son">
字体大小 1.2 * 14（根节点html） = 16px
<div class="rem-grandson">
字体大小 1.2 * 14（根节点html） = 16px
</div>
</div>
</div>
<br>
<h1>rem 也可作为固定长度单位设置宽高等</h1>
<div class="rem-box">
宽：14 * 10 = 140px<br>
高：14 * 10 = 140px
</div>
<br>
<h1>vh,vw 屏幕可见区域的高度，宽度的1%</h1>
<div class="vhvw-box">
宽：屏幕宽度的50%<br>
高：屏幕高度的50%
</div>
</body>
</html>

<!DOCTYPE html>
<html lang="Zh-cn">
<head>
<meta charset="UTF-8">
<title>一程烟雨</title>
</head>
<style type="text/css" media="screen">
html{
font-size: 14px;
}
.em,
.em > .em-son,
.em > .em-son > .em-grandson {
font-size: 1.2em;
}
.rem,
.rem > .rem-son,
.rem > .rem-son > .rem-grandson {
font-size: 1.2rem;
}
.rem-box {
background: #d60b3b;
width:10rem;
height: 10rem;
color: #fff;
text-align: center;
line-height:5rem;
}
.vhvw-box {
background: #d60b3b;
width:50vw;
height: 50vh;
color: #fff;
text-align: center;
line-height:25vh;
}
</style>
<body>
<h1>em 继承父元素的字体大小，来变大或变小，多层嵌套字体变化</h1>
<div class="em">
字体大小 1.2 * 14（父元素body） = 16px
<div class="em-son">
字体大小 1.2 * 16(父元素em) = 20px
<div class="em-grandson">
字体大小 1.2 * 20(父元素em-son) = 24px
</div>
</div>
</div>
<br>
<h1>rem 继承根节点元素的字体大小，来变大或变小，多层嵌套字体不变化</h1>
<div class="rem">
字体大小 1.2 * 14（根节点html） = 16px
<div class="rem-son">
字体大小 1.2 * 14（根节点html） = 16px
<div class="rem-grandson">
字体大小 1.2 * 14（根节点html） = 16px
</div>
</div>
</div>
<br>
<h1>rem 也可作为固定长度单位设置宽高等</h1>
<div class="rem-box">
宽：14 * 10 = 140px<br>
高：14 * 10 = 140px
</div>
<br>
<h1>vh,vw 屏幕可见区域的高度，宽度的1%</h1>
<div class="vhvw-box">
宽：屏幕宽度的50%<br>
高：屏幕高度的50%
</div>
</body>
</html>
```