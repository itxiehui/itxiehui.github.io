---
title: JS之事件详解（二）
date: 2016-05-19 17:10:00
tags: [编程,javacript]
---

本文章是本人在学习js时候的知识总结，涉及到学习过程中的一些知识点，代码练习以及新手常犯的错误与解决方法，希望对js的爱好者有帮助。

<!--more-->

## **键盘事件**
***
### keyCode
> 键盘和鼠标的键都有一个相应的键码对应，利用onkeydown函数可以调用
其他属性还有：ctrlKey，shiftKey，altKey

### 练习代码
#### 1.获取用户按下键盘哪个键
```javascript
<script>
	window.onload = function(){
		document.onkeydown= function(ev){
			var oEvent = ev || event;
			alert(oEvent.keyCode);
			//左键 37 ；上 38 ；右 39 ；下 40 ；enter 13
		}
	}
	</script>
```
#### 2.键盘控制div移动
```javascript
<style>
		#div1{
			width: 100px;
			height: 100px;
			background-color: #311245;
			position: absolute;
		}

	</style>
	<script>
	window.onload = function(){
		var oDiv = document.getElementById('div1');

		document.onkeydown = function(ev){
			var oEvent = ev || event;
			var keyCode = oEvent.keyCode;

			switch(keyCode){
				case 37:oDiv.style.left = oDiv.offsetLeft-10+'px';break;
				case 39:oDiv.style.left = oDiv.offsetLeft+10+'px';break;
				case 38:oDiv.style.top = oDiv.offsetTop-10+'px';break;
				case 40:oDiv.style.top = oDiv.offsetTop+10+'px';break;
			}
		}
	}
	</script>
</head>
<body>
	<div id="div1"></div>
</body>
```

 * -解决卡顿问题
>  要点：1.开定时器 2.加判断
		
```javascript
<style>
		#div1{
			width: 100px;
			height: 100px;
			background-color: #311245;
			position: absolute;
		}

	</style>
	<script>
	window.onload = function(){
		var oDiv = document.getElementById('div1');
		var timer = null;
		var left = false;
		var top = false;
		var bottom = false;
		var right = false;

		setInterval(function(){
			if (left) {
				oDiv.style.left = oDiv.offsetLeft-10+'px';
			}else if(right){
				oDiv.style.left = oDiv.offsetLeft+10+'px';
			}else if(top){
				oDiv.style.top = oDiv.offsetTop-10+'px';
			}else if(bottom){
				oDiv.style.top = oDiv.offsetTop+10+'px';
			}
		},50)

		document.onkeydown = function(ev){
			var oEvent = ev || event;
			var keyCode = oEvent.keyCode;

			switch(keyCode){
				case 37:left = true;break;
				case 39:right = true;break;
				case 38:top = true;break;
				case 40:bottom = true;break;
			}
		}
		document.onkeyup = function(ev){
			var oEvent = ev || event;
			var keyCode = oEvent.keyCode;

			switch(keyCode){
				case 37:left = false;break;
				case 39:right = false;break;
				case 38:top = false;break;
				case 40:bottom = false;break;
			}
		}
	}
	</script>
</head>
<body>
	<div id="div1"></div>
</body>
```
#### 3.提交留言
* 回车提交
```javascript
<script>
	window.onload =function(){
		var oTxt1 = document.getElementById('txt1');
		var oTxt2 = document.getElementById('txt2');
		var oBtn = document.getElementById('btn');

		var commit = btn.onclick = function(){
			oTxt1.value += oTxt2.value + '\n';
			oTxt2.value = '';
		}
		document.onkeydown=function(ev){
			var oEvent = ev || event;
			commit();
		}
	}
	</script>
</head>
<body>
	<textarea name="" id="txt1" cols="30" rows="10" ></textarea><br>
	<input type="text" id="txt2"><button id="btn">提交</button>
</body>
```
* ctrl+回车 提交
> 在上面的基础上，更改if条件
```javascript
if (oEvent.ctrlKey == true && oEvent.keyCode == 13) {
				commit();
			};
```

><span style="font-size:12px">本文标题: <a href="{{ permalink }}">  {{title}}  </a>
>文章作者: <a href="http://itxiehui.github.io/">劳土铸</a>  
>许可协议: <img alt="知识共享许可协议" style="border-width:0" src="https://i.creativecommons.org/l/by-nc-sa/4.0/80x15.png" /><a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/">?署名-非商用-相同方式共享 4.0</a></span>
 
