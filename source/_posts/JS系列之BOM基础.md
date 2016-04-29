---
title: js系列之DOM基础
date: 
tags: [编程,javacript,BOM]
---

本文章是本人在学习js时候的知识总结，涉及到学习过程中的一些知识点，代码练习以及新手常犯的错误与解决方法，希望对js的爱好者有帮助。

<!--more-->

# js系列之BOM基础
***
## open

> window.open(href,target); target参数为可选参数，可以填‘_blank’(在新窗口打开)，‘_self’(在当前窗口打开)；有返回值，返回窗口本身

* 在文本窗口写代码，新窗口打开执行
 ```javascript
 <script>
	window.onload = function(){
		var oBtn = document.getElementById('btn1');
		var oTxt = document.getElementById('txt1');

		oBtn.onclick = function(){
			var oNewWin = window.open('about:blank');
			oNewWin.document.write(oTxt.value);
		}
	}
</script>
<body>
	<textarea name="" id="txt1" cols="30" rows="10"></textarea>
	<br>
	<button id="btn1" value="运行">运行</button>
</body>
```

## close

> close的页面需要是有js打开的页面
比如：
在open.html上 关键js代码：window.open(‘close.html’,’_blank’);
在close.html上 关键js代码：window.close();
close.html是js代码打开的页面

## userAgent

> 查看浏览器版本
alert（window.navigator.userAgent）;

```javascript
<script>
	alert(window.navigator.userAgent)//查看浏览器的版本
</script>
```

## location

> window.location   当前页面的地址
window.location ='http://www.github.com';   打开一个新地址


## 系统对话框：
> 警告框：alert（） ,没有返回值
选择框：confirm（“提问的内容”），返回boolen
输入框：prompt（“问题”，“默认值”）。返回输入的字符串或null。

 
## 窗口尺寸，工作区尺寸
> 可视区尺寸
document.documentElement.clientWidth
document.documentElement.clientHeight
滚动距离
document.body.scrollTop
document.documentElement.scrollTop	*这两句是为了更好的兼容性，chrome和firefox只认识前者*

* 侧边栏
 ```javascript
 <style>
	#div1{
		width: 100px;
		height: 100px;
		position: absolute;
		right: 0;
		background: pink;
	}
</style>
<script>
window.onresize = window.onload = window.onscroll  = function (){
		var oDiv = document.getElementById('div1');
		var scrollTop = document.documentElement.scrollTop || document.body.scrollTop;
		var t = (document.documentElement.clientHeight-oDiv.offsetHeight)/2;

		oDiv.style.top = scrollTop + t +'px';
	}
</script>
<body style="height:1000px;">
	<div id="div1"></div>
</body>
```

* 回到顶部
 ```javascript
<style>
	#btn{
		position: fixed;
		bottom: 0;
		right: 0;
	}
</style>
<script>
	window.onload = function(){
		var oBtn = document.getElementById('btn');
		var timer =null;
		var bSys = null;

		//如何检测到用户操作了滚动条
		window.onscroll = function(){
			if (!bSys) {
				clearInterval(timer);
			};
			bSys = false;
		}
		oBtn.onclick = function(){

			timer =setInterval(function(){
				var scrollTop = document.documentElement.scrollTop || document.body.scrollTop;
				var iSpeed = Math.floor(-scrollTop/8);
				if (scrollTop == 0) {
					clearInterval(timer);
				};
				document.documentElement.scrollTop = scrollTop +iSpeed;
				bSys = true;
			},30)
			
		}
	}
</script>
<body>
	<button id="btn">回到顶部</button>
	<p>**</p>
	<p>**</p>
	<p>**</p>
	<p>**</p>
	...
</body>
 ```
 
> **小结**
>以上是JavaScript中BOM（Browsert object model）的一些常用的基础，以及帮助理解的练习代码


><span style="font-size:12px">本文标题: <a href="{{ permalink }}">  { {title} }  </a>
>文章作者: <a href="http://itxiehui.github.io/">劳土铸</a>  
>许可协议: <img alt="知识共享许可协议" style="border-width:0" src="https://i.creativecommons.org/l/by-nc-sa/4.0/80x15.png" /><a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/">?署名-非商用-相同方式共享 4.0</a></span>
 
