---
title: link 和@import 的区别
date: 2018-10-14 11:25:28
tags: [HTML5,CSS,编程,Web]
---
在学习HTML和CSS的一些总结和经验，供大家参考和学习，同时也欢迎大家参与讨论。

<!--more-->

>**前言：**这个问题也是在前端面试经常会问到的的题目。

# 用法
### 比如：
#### 1、@import
```javascript
	<style type="text/css" media="screen">
	@import url("http://www.taobao.com/home/css/global/v2.0.css?t=20070518.css");
	</style>
```
#### 2、link
```javascript
	<link rel="stylesheet" rev="stylesheet" href="default.css" type="text/css" media="all" />
```

# 差别
### 外部引用CSS中 link与@import的区别

>**ONE：**link属于XHTML标签，而@import完全是CSS提供的一种方式。link标签除了可以加载CSS外，还可以做很多其它的事情，比如定义RSS，定义rel连接属性等，@import就只能加载CSS了。

----------
>**TWO：**加载顺序的差别。当一个页面被加载的时候，link引用的CSS会同时被加载，而@import引用的CSS 会等到页面全部被下载完再被加载。所以有时候浏览@import加载CSS的页面时开始会没有样式（就是闪烁），网速慢的时候比较明显。

------------
>**THREE：**兼容性的差别。由于@import是CSS2.1提出的，所以老的浏览器不支持，@import只有在IE5以上的才能识别，而link标签无此问题，都支持。

-----------
>**FOUR：**使用DOM控制样式时的差别。当使用JavaScript控制DOM去改变样式的时候，只能使用link标签，因为@import不是DOM可以控制的。

-----------
>**FIVE：**link方式的样式的权重高于@import的权重。

------------
>**SIX：**link可以使用 js 动态引入，@import不行。

----------------

><span style="font-size:12px">文章标题: <a href="{{permalink}}">{{ title }}</a>
文章作者: <a href="http://itxiehui.github.io/">王奕聪，QQ:1301842163</a>  
许可协议: <img src="https://i.creativecommons.org/l/by-nc-sa/4.0/80x15.png" style="border-width: 0;" alt="知识共享许可协议"   />
<a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/">©署名-非商用-相同方式共享 4.0</a></span>