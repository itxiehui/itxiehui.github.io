---
title: js系列之DOM基础
date: 2016-04-23 13:55:00
tags: [编程,javacript,DOM]
---

本文章是本人在学习js时候的知识总结，涉及到学习过程中的一些知识点，代码练习以及新手常犯的错误与解决方法，希望对js的爱好者有帮助。

<!--more-->

## DOM节点

### childNodes，nodeType和children

* childNodes子节点，只适合用于旧版的ie浏览器，用于火狐谷歌上会将文本节点（空格）包括进来。

* nodeType节点类型，返回以一个数字。1表示元素节点，2指属性节点，3是文本节点。

* children作用同于childNodes，但强于它，是它的兼容版，返回只包含元素节点的数据。

### 练习代码：

```javascript
<script>

window.onload= function(){

var oUl = document.getElementById('ul1');

var i=0;

alert(oUl.childNodes.length)//无论在火狐或者ie，360，谷歌都显示11

/* for(i=0; i<oUl.childNodes.length;i++)

{

//oUl.childNodes[i].style.background == 'red'

if (oUl.childNodes[i].nodeType == 1)

{

oUl.childNodes[i].style.background = 'red';

};

}

alert(oUl.children.length) //children是childNodes的兼容版*/

}

</script>

<body>

<ul id="ul1">

<li>111</li>

<li>222</li>

<li>333</li>

<li>444</li>

<li>555</li>

</ul>

</body>
```

### parentNode

```javascript
<script>

window.onload = function(){

var oUl = document.getElementById('ul1');

var i;

var aA = oUl.getElementsByTagName('a');

for(i=0;i< aA.length; i++){

aA[i].onclick = function(){

this.parentNode.style.display = 'none';

}

}

};

</script>

<body>

<ul id="ul1">

<li>jjjj<a href="javascript:;">隐藏</a></li> <!--避免a链接点击立即刷新来不及显示效果-->

<li>啊啊j<a href="javascript:">隐藏</a></li>

<li>的<a href="javascript:">隐藏</a></li>

<li>j得到j<a href="javascript:">隐藏</a></li>

</ul>

</body>
```

### offsetParent

* offsetParent属性返回一个对象的引用，这个对象是距离调用offsetParent的元素最近的（在包含层次中最靠近的），并且是已进行过CSS定位的容器元素。 如果这个容器元素未进行CSS定位, 则offsetParent属性的取值为根元素(在标准兼容模式下为html元素；在怪异呈现模式下为body元素)的引用。 当容器元素的style.display 被设置为 "none"时（译注：IE和Opera除外），offsetParent属性 返回 null。一般返回设置了定位元素的父元素

```javascript
<body>

<div id="div1" style="width:100px; height:100px;background:red; position:relative;margin:100px;">

<div id="div2" onclick="alert(this.offsetParent.id)" style="width:100px; height:100px;background:yellow; position:absolute; top:100px;left:100px;"></div></div>

</body>
```

### firstChild与firstElementChild

* firstChild在火狐谷歌下面都是undefined，出错；以前firstChild只能在ie下有用，现在获取首节点用firstElementChild，同理可延伸到lastChild和lastElementChild

* 还有兄弟节点，nextSibling与nextElementSibling 和previousSibling与previousElementSibling

```javascript
<script type="text/javascript">

window.onload = function(){

var oUl = document.getElementById('ul1');

var i;

//firstChild在火狐谷歌下面都是undefined，出错；以前firstChild只能在ie下有用

//oUl.firstChild.style.background = 'red';

//新一代firstElementChild兼容版

oUl.firstElementChild.style.background = 'red';

//以前解决这一问题的方法

oFirst = oUl.firstElementChild || oUl.firstChild

oFirst.style.background = 'red'

//同理也有lastChild 和 lastElementChild

//兄弟节点

var oLi = document.getElementById('li1');

//oLi.nextSibling.style.background = 'red';

oLi.nextElementSibling.style.background = 'red';

//同理nextSibling nextElementSibling和previousSibling previousElementSibling亦是同样的用法

}

</script>

<body>

<ul id="ul1">

<li>111</li>

<li>222</li>

<li id="li1">333</li>

<li>444</li>

<li>555</li>

</ul>
```

## DOM方法操作元素属性

***
```javascript   
<script>

window.onload = function(){

oTxt = document.getElementById('txt1');

//1

//oTxt.value = 'asdd';

//2

//oTxt['value'] = 'aswerr';

//3

oTxt.setAttribute("value", "jhgf");

alert(oTxt.getAttribute('id'));

}

</script>

<body>

<input type="text" id="txt1">

</body>
```

## DOM元素灵活查找

### className

* 通过class选取元素，一般分两步：
  1. 把所有的子元素选出来
  2. 用className作为条件筛选出来处理

```javascript
<script>

//通过class来选元素

window.onload = function (){

var oUl = document.getElementById('ul1');

var aLi = oUl.getElementsByTagName('li');

var i;

for (var i = 0; i < aLi.length; i++) {

if (aLi[i].className == 'double') {

aLi[i].style.background = 'green';

};

};

}

</script>

<body>

<ul id="ul1">

<li></li>

<li class="double"></li>

<li></li>

<li class="double"></li>

<li></li>

<li class="double"></li>

</ul>

</body>
```

**进一步改进**，写一个函数完成选class的功能

```javacript
//通过class来选元素的函数

function getByClass(oParent, sClass){

var aEle =oParent.getElementsByTagName('*');

var aResult = new Array();

for (var i = 0; i < aEle.length; i++) {

if (aEle[i].className == sClass) { //注意千万不要写成‘sClass’，因为传过时就已经带有‘’了

aResult.push(aEle[i]);

};

};

return aResult;

};
```

引用如下：

```javascript
window.onload = function (){

var oUl = document.getElementById('ul1');

var aDou = getByClass(oUl,'double');

var i;

for (i = 0; i < aDou.length; i++) {

aDou[i].style.background = 'yellow';

};

}

</script>

<body>

<ul id="ul1">

<li></li>

<li class="double"></li>

<li></li>

<li class="double"></li>

<li></li>

<li class="double"></li>

</ul>

</body>
```

## 创建、插入、和删除元素

***

### 创建DOM元素

* creatElement（标签名）  创建一个节点
  * appendChild（节点）	追加一个节点
    -例子：为ul插入li

### 插入元素

* insertBefore（节点，原有节点）		在已有元素前插入
  -例子：倒序插入li

### 删除DOM元素

* removeChild（节点）		删除一个元素
  -例子删除li

### 练习代码：

①创建插入li 

```javascript
<script>
	window.onload = function(){
		oUl = document.getElementById('ul1');
		oBtn = document.getElementById('btn1');
		oTxt = document.getElementById('txt1');

		oBtn.onclick = function(){
			var oLi = document.createElement('li');
			var aLi = oUl.getElementsByTagName('li');

			oLi.innerHTML = oTxt.value;
			if (aLi.length == 0) {
				oUl.appendChild(oLi);
			}else{
				oUl.insertBefore(oLi, aLi[0]);
			}

			
		}
	}
</script>
<body>
	<label for="txt1"></label><input type="text" id="txt1">
	<button id="btn1">插入</button>

	<ul id="ul1"></ul>
</body>
```

**注意**：appendchild(创建元素)，创建子节点；insertBefore（插入的对象，在哪里之前插入）
​	
②删除元素
```javascript   
<script>
	window.onload = function(){
		var oUl = document.getElementById('ul1');
		var aA = document.getElementsByTagName('a');
		var i;

		for (i = 0; i < aA.length; i++) {
			aA[i].onclick = function (){
				oUl.removeChild(this.parentNode);
			}
		};
	}
</script>
<body>
	<ul id="ul1">
		<li>li1<a href="javascript:;">删除</a></li>
		<li>li2<a href="javascript:;">删除</a></li>
		<li>li3<a href="javascript:;">删除</a></li>
		<li>li4<a href="javascript:;">删除</a></li>
		<li>li5<a href="javascript:;">删除</a></li>
	</ul>
</body>
```

**注意**：a标签里href属性值为JavaScript：； 作用为取消默认行为，避免跳转刷新，相当一个js入口，不写的话js代码没有效果。

>**小结**
>以上是JavaScript中DOM（document object model）的一些基础，覆盖面不是很广，想了解更多的朋友可以参考[wschoolDOM参考手册](http://www.w3school.com.cn/htmldom/index.asp)或者阅读[JavaScript DOM编程艺术（第二版）](http://www.javascriptcn.com/read-42.html)一书



><span style="font-size:12px">本文标题: <a href="{{ permalink }}">  { {title} }  </a>
>文章作者: <a href="http://itxiehui.github.io/">劳土铸</a>  
>许可协议: <img alt="知识共享许可协议" style="border-width:0" src="https://i.creativecommons.org/l/by-nc-sa/4.0/80x15.png" /><a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/">?署名-非商用-相同方式共享 4.0</a></span>

