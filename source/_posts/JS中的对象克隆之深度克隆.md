---
title: JS中的对象克隆之深度克隆(拷贝)
date: 2019-03-13 16:44:28
tags: [JavaScript,编程,Web]
---
在学习JavaScript的一些总结和经验，供大家参考和学习，同时也欢迎大家参与讨论。

<!--more-->
# 2. 深度克隆(拷贝)
>**前言：**在上一篇文章开头的介绍中我们是不是说过对一个**对象**进行修改不会影响到另一个对象，但是很明显，在**[浅度克隆](https://zsk.gdinit.cn/2018/08/28/JS%E4%B8%AD%E7%9A%84%E5%AF%B9%E8%B1%A1%E5%85%8B%E9%9A%86%EF%BC%88%E4%B8%80%EF%BC%89/)**中对**obj1**中的**sex**改动的时候（因为sex是数组，是引用类型，为堆内存中的对象），**obj2**中的**sex**也随着改动了，这样就达不到我们想要的效果，所以**深度克隆**就是来解决这个问题的。

- # **前提** :  首先，深度克隆会稍微比较难一点，所以读这篇文章你要理解一下JavaScript中的**原型**，还有以下几个方法的应用。

## Object.hasOwnProperty(prop)
## Object.prototype.toString()
## Function.prototype.apply()
## Function.prototype.call()

>至于如何应用，那就要读者自己去了解了,可以到各大博客社区或者**MDN,javaScript高程设计**看看

-------------------------
>例子：

```javascript
<script type="text/javascript">
	//深度克隆:
	var obj = {
		name : 'abc',//（原始值）
		age : 18,//（原始值）
		card : ['visa','master'],//数组(引用值)
		wife : {//对象(引用值)
			name : 'Tom',
			son : {
				name: 'mySon'
			}
		}
	};
	var obj1 = {};
//遍历对象 for (var prop in obj) 数组也能for in (其中prop为索引值)
//1、判断是不是原始值typeof()
//2、判断是数组还是对象      
//3、建立相应的数组或对象

function deepClone(origin,target) {
	var target = target || {},
		toStr = Object.prototype.toString,//使用toString()检测对象类型
		arrStr = '[object Array]';
	for(var prop in origin){
		if (origin.hasOwnProperty(prop)) {//判断是不是原型上面的,如果是返回false
			if (origin[prop] !== 'null' && typeof(origin[prop]) == 'object') {
				if(toStr.call(origin[prop]) == arrStr){
				//判断是不是数组[object Array]
					target[prop] = [];
				}else{
					target[prop] = {};
				}
				deepClone(origin[prop],target[prop]);//递归操作
			}else{
				target[prop] = origin[prop];
			}
		}
	}
	return target;
}

//利用三目运算符简化代码，如下：
	function deepClone(origin,target) {
	var target = target || {},
		toStr = Object.prototype.toString,
		arrStr = '[object Array]';
	for(var prop in origin){//遍历对象
		if (origin.hasOwnProperty(prop)) {//判断是不是原型上面的,如果是返回false
			if (origin[prop] !== 'null' && typeof(origin[prop]) == 'object') {
				target[prop] = toStr.call(origin[prop]) == arrStr? [] : {};
				deepClone(origin[prop],target[prop]);//递归操作
			}else{
				target[prop] = origin[prop];
			}
		}
	}
	return target;
}
</script>
```

## 简化代码：
>利用JSON对象中的**stringify()和parse()**方法
```javascript
<script type="text/javascript">
	var book = {
	    title: "professional javaScript",
	    authors: [
	        "yicong",
	        "wangyicong"
	    ],
	    edition: 3,
	    year: 2019,
	    name: undefined,//不会输出，只输出有效的JSON数据类型的实例属性
	};
	
	var copyBook = JSON.parse(JSON.stringify(book));
	console.log(copyBook);
	book.title = "我是一个基础数据类型";
	book.authors.push("我是一个引用数据类型");
	console.log(book);
</script>
```

----------------

><span style="font-size:12px">文章标题: <a href="{{permalink}}">{{ title }}</a>
文章作者: <a href="http://itxiehui.github.io/">王奕聪，QQ:1301842163</a>  
许可协议: <img src="https://i.creativecommons.org/l/by-nc-sa/4.0/80x15.png" style="border-width: 0;" alt="知识共享许可协议"   />
<a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/">©署名-非商用-相同方式共享 4.0</a></span>