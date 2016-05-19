---
title: JS之默认行为
date: 2016-05-19 17:10:00
tags: [编程,javacript]
---

本文章是本人在学习js时候的知识总结，涉及到学习过程中的一些知识点，代码练习以及新手常犯的错误与解决方法，希望对js的爱好者有帮助。

<!--more-->

## **默认行为**
***

> 浏览器自带的事件，比如：右键菜单，a链接跳转，表单提交
return false;取消默认行为

### 练习代码
#### 1.屏蔽右键菜单
> --弹出自定义的右键菜单
```javascipt
<style>
	*{
		margin: 0;
		padding:0;
	}
		#ul1{
			width: 130px;
			height: 205px;
			background-color: #ccc;
			list-style: none;
			position: absolute;
			text-indent: 10px;
			display: none;
		}
		#ul1 li{
			padding-bottom: 10px;
			border-bottom: 1px solid #000;
			height: 30px;
			line-height: 30px;

		}
	</style>
	<script>
	window.onload = function(){
		var oUl = document.getElementById('ul1');

		document.oncontextmenu = function(ev){
			var oEvent = ev || event;

			oUl.style.display = 'block';
			oUl.style.left = oEvent.clientX +'px';
			oUl.style.top = oEvent.clientY+'px';


			return false;
		}

		document.onclick =function(){
			var oUl = document.getElementById('ul1');
			oUl.style.display = 'none';
			
		}
	}
	</script>
</head>
<body>
	<ul id="ul1">
		<li>返回</li>
		<li>刷新</li>
		<li>开发者模式</li>
		<li>查看源代码</li>
		<li>属性</li>
	</ul>
</body>
```
#### 2.拖拽div（简易版）
> 原理：鼠标和div左上角顶点距离不变；三个函数，onmousedown（存储距离），onmousemove（计算鼠标移动后的位置），onmouseup（取消函数），后二者在前者里面
```javascript
<style>
		#div1{
			width: 100px;
			height: 100px;
			background-color: red;
			position: absolute;
		}
	</style>
	<script>
		window.onload =function(){
			var oDiv = document.getElementById('div1');
			var disX =0;
			var disY =0;

			oDiv.onmousedown = function(ev){
				var oEvent = ev || event;

				disX = oEvent.clientX- oDiv.offsetLeft;
				disY = oEvent.clientY - oDiv.offsetTop;

				oDiv.onmousemove=function(ev){
					var oEvent = ev || event;

					oDiv.style.left = oEvent.clientX-disX+'px';
					oDiv.style.top = oEvent.clientY-disY+'px';
				}

				oDiv.onmouseup = function(){
					oDiv.onmousemove = null;
					oDiv.onmouseup = null;
				}
			return false； //解决旧版火狐的bug
			}
		}
	</script>
</head>
<body>
	<div id="div1"></div>
</body>
```

* 解决div拖动时，鼠标移出问题
	> 原因：鼠标拖得太快，脱离div事件停止调用，想办法扩大事件范围，把div的onmousemove和onmouseup事件改为document的
```javascript
document.onmousemove=function(ev){
					var oEvent = ev || event;

					oDiv.style.left = oEvent.clientX-disX+'px';
					oDiv.style.top = oEvent.clientY-disY+'px';
				}

				document.onmouseup = function(){
					document.onmousemove = null;
					document.onmouseup = null;
				}
```	
* 解决div拖出可视区
```javascript
document.onmousemove=function(ev){
					var oEvent = ev || event;

					var l = oEvent.clientX-disX;
					var t = oEvent.clientY-disY;

					if (l<0) {  //限制div移出可视区
						l=0;
					}else if(l>document.documentElement.clientWidth- oDiv.offsetWidth){
						l = document.documentElement.clientWidth- oDiv.offsetWidth;
					};
					if (t<0) {
						t = 0;
					}else if(t>document.documentElement.clientHeight- oDiv.offsetHeight){
						t =document.documentElement.clientHeight- oDiv.offsetHeight;
					}

					oDiv.style.left = l+'px';
					oDiv.style.top = t+'px';
				}

```

><span style="font-size:12px">本文标题: <a href="{{ permalink }}">  {{title}}  </a>
>文章作者: <a href="http://itxiehui.github.io/">劳土铸</a>  
>许可协议: <img alt="知识共享许可协议" style="border-width:0" src="https://i.creativecommons.org/l/by-nc-sa/4.0/80x15.png" /><a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/">?署名-非商用-相同方式共享 4.0</a></span>
 
