---
title: JS中的对象克隆（拷贝）
date: 2018-08-28 10:48:28
tags: [JavaScript,编程,Web]
---
在学习JavaScript的一些总结和经验，供大家参考和学习，同时也欢迎大家参与讨论。

<!--more-->

## 什么是克隆(拷贝)？
>**克隆**是在JavaScript中必学的一个知识点，也是比较常见的问题。克隆(拷贝)就是将一个对象(**obj1**)里面的属性和方法等复制到另一个对象(**obj2**),且克隆之后，对一个对象进行改动，不会影响到另一对象，就是两者互不影响，还是两个不同的个体。
它分为**浅度克隆**和**深度克隆**两种，下面将对这两种进行解释和讨论。

# 1. 浅度克隆

``` javascript
	<script type="text/javascript">
		var obj1 = {
			name : 'IT协会',
			age : 'the Tenth',
			sex : ['male','female']
		}
		function shallowClone(prpo){
			var temp = {};           //var 一个临时空对象
			for (var i in prpo){     //利用for...in函数进行遍历对象
				temp[i] = prpo[i];
			}
			return temp;             //返回temp对象
		}
		var obj2 = shallowClone(obj1);
	</script>
```
>上述的代码就是将**obj1**对象中的**name,age,sex**复制一份到**obj2**,我们可以在浏览器的控制台看一下，如下：

![Alt text](https://wx1.sinaimg.cn/mw690/007d7DTvly1fusvwcis7bj30k4098dg4.jpg)
>从上述可以看到**obj2**中也有了与**obj1**一样的属性和**sex**数组，那现在我们就想如果我改变一下**obj1**中的属性或数组会有什么变化呢？接下来我们来操作一下：

```javascript
	<script type="text/javascript">
		var obj1 = {
			name : 'IT协会',
			age : 'the Tenth',
			sex : ['male','female']
		}
		function shallowClone(prpo){
			var temp = {}; //var 一个临时空对象
			for (var i in prpo){ //利用for...in函数进行遍历对象
				temp[i] = prpo[i];
			}
			return temp;//返回temp对象
		}
		var obj2 = shallowClone(obj1);
		obj1.name = '信息技术协会';
		obj1.age = 10;
		obj1.sex.push('middle');
	</script>
```

		
		
>上面我只改变了obj1中的属性和数组，那我们想一下obj2是否会发生变化呢？接下来我们来看一下输出是怎样的：

![Alt text](https://wx1.sinaimg.cn/mw690/007d7DTvly1fusvwciuecj30k5094wev.jpg)

>我们可以看出来，**obj1**中的的**age和name、sex**均变化了，但是为什么在**obj2**中为什么**sex**会跟着变化呢，而**age和name**不会？


### 说到这里我们就要来理解一下基本数据类型和引用值类型

>我们知道JavaScript中的基本数据数据类型有**Undefined、Null、Boolean、Number和String**5种，引用类型有**Object、Array、Date、RegExp、Function、包装类**等，我们学过**数据结构与算法**的同学应该知道**栈内存和堆内存**的区别。
* 基本数据类型是存放在栈内存中的，引用类型是堆内存中的对象

----------
### **原因**：
> name和age的值属于**基本类型**，所以拷贝的时候传递的就是该数据段；但是sex的值是堆内存中的对象，所以sex在拷贝的时候传递的是**指向sex对象的地址**，无论复制多少个sex，其值始终是指向父对象的sex对象的**内存空间**，如下图：

![Alt text](https://wx2.sinaimg.cn/mw690/007d7DTvly1fuu5urmxxfj30li0daweo.jpg)

### 简洁代码（另外一种方法）：
#### 使用Object构造函数的方法Object.assign()
>通过复制一个或多个对象来创建一个新的对象。**Object.assign()**用于将所有可枚举属性的值从一个或多个源对象复制到目标对象。它将返回目标对象。
#### 语法：Object.assign(target, ...sources)
```javascript
	var obj1 = {
		name : 'IT协会',
		age : 'the Tenth',
		sex : ['male','female']
	}
	var another = {
		name:"yicong",
		age: 18,
		friend: ["Tom","john"]
	}
	var obj2 = Object.assign({},obj1,another);
	console.log(obj2);
	obj1.name = "信息技术协会";
	obj1.sex.push("middle");
	console.log(obj1);
```

----------

										深度克隆请看下一篇 

----------------

><span style="font-size:12px">文章标题: <a href="{{permalink}}">{{ title }}</a>
文章作者: <a href="http://itxiehui.github.io/">王奕聪，QQ:1301842163</a>  
许可协议: <img src="https://i.creativecommons.org/l/by-nc-sa/4.0/80x15.png" style="border-width: 0;" alt="知识共享许可协议"   />
<a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/">©署名-非商用-相同方式共享 4.0</a></span>