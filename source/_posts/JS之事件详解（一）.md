---
title: JS之事件详解（一）
date: 2016-05-19 17:10:00
tags: [编程,javacript]
---

本文章是本人在学习js时候的知识总结，涉及到学习过程中的一些知识点，代码练习以及新手常犯的错误与解决方法，希望对js的爱好者有帮助。

<!--more-->

## **event对象**
 
> * Event代表事件状态，用来获取事件的详细信息，如事件发生的元素，键盘按键，鼠标位置和鼠标按钮状态。一旦事件发生,便会生成Event对象，如单击一个按钮，浏览器的内存中就产生相应的 event对象。
event对象只在事件发生的过程中才有效。
event的某些属性只对特定的事件有意义。比如，fromElement 和 toElement 属性只对 onmouseover 和 onmouseout 事件有意义。

> * 获取鼠标的位置：clientX，clientY

> * 获取event对象的兼容性写法：var oEvent = ev || event；
***
> * 事件冒泡：一个对象触发了某类事件，该事件程序被执行了之后，这个事件还会向这个对象的父级元素传播，从而会触发父级的事件程序。
一般来说会造成困扰，解决方法：oEvent.cancelBubble = true;

###  **练习代码**
#### 1.弹出菜单
    *取消事件冒泡*
```javascript
	<style>
	*{
		margin: 0;
		padding:0;

	}
	ul{
		background-color: #1457dd;
		padding:5px 10px;
		display:none ;
		list-style: none;
		width: 150px;
	}
	div{
		position: absolute;
		left:45%;
		top:10%;
	}
	</style>

	<script>
	window.onload = function(){

	var oBtn = document.getElementById('btn1');
	var oUl = document.getElementById('ul1');


	oBtn.onclick =function (ev){
		var oEvent = ev || event;
		oUl.style.display = 'block';

		oEvent.cancelBubble = 'true'

	}
	document.onclick = function(){
		oUl.style.display = 'none';
		
	}
	}
	</script>
</head>
<body>
	<div>
		<button id="btn1">显示</button>
		<ul id="ul1">
			<li>收藏</li>
			<li>刷新</li>
			<li>注销</li>
			<li>登陆</li>
			<li>注册</li>
		</ul>
	</div>
	
</body
```
#### 2.跟随鼠标移动的div
    *注意：消除滚动条的影响*
```javascript
<style>
		#div1{
			width: 100px;
			height: 100px;
			background-color: #120047;
			position: absolute; //别忘记加这个属性，有了定位left和top才能设置
		}
	</style>
	<script>
	document.onmousemove = function(ev){
		var oDiv = document.getElementById('div1');
		var oEvent = ev || event; //event在ie下才有定义
		var scrollTop = document.documentElement.scrollTop || document.body.scrollTop;
		var scrollLeft = document.documentElement.scrollLeft || document.body.scrollLeft;

		oDiv.style.left = oEvent.clientX +scrollLeft+'px';
		oDiv.style.top = oEvent.clientY + scrollTop+'px' ;
	
	
	}
		
	</script>
</head>
<body>
	<div id="div1"></div>
</body>
```

#### 3.获取鼠标在页面的绝对位置（一串跟随鼠标的div）
```javascript
	<style>
		div{
			width: 10px;
			height: 10px;
			background-color: #124888;
			position: absolute;
		}
	</style>

	<script>
	window.onload = function(){
		var aDiv = document.getElementsByTagName('div');
		var i = 0;

		document.onmousemove =function (ev){
			var oEvent = ev || event;

			for(i=aDiv.length-1; i>0; i--){
				aDiv[i].style.left = aDiv[i-1].style.left;
				aDiv[i].style.top = aDiv[i-1].style.top;
			}

			aDiv[0].style.left = oEvent.clientX + 'px';
			aDiv[0].style.top = oEvent.clientY + 'px';

		}
	}
	</script>
</head>
<body>
	Div*50
</body>
```

><span style="font-size:12px">本文标题: <a href="{{ permalink }}">  {{title}}  </a>
>文章作者: <a href="http://itxiehui.github.io/">劳土铸</a>  
>许可协议: <img alt="知识共享许可协议" style="border-width:0" src="https://i.creativecommons.org/l/by-nc-sa/4.0/80x15.png" /><a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/">?署名-非商用-相同方式共享 4.0</a></span>
 
