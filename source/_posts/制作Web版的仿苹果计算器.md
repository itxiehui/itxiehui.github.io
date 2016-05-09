---
title: 制作Web版的仿苹果计算器
date: 2016-05-09 17:52:40
tags: [集训,编程,Web]
---

**项目源代码下载**：[http://pan.baidu.com/s/1skRXs5Z](http://pan.baidu.com/s/1skRXs5Z) 

<!--more-->

## 一、用本项目剖析相关知识

## ![](http://ww3.sinaimg.cn/mw690/006rmJyDgw1f3p5hvkxzpj30bp0jl3yz.jpg)

浏览项目结构、代码

这其实就是一个简单的Web应用（插入：Web应用、手机软件、桌面软件）

**网页三剑客**

![](http://ww4.sinaimg.cn/mw690/006rmJyDgw1f3p5hvwoxnj30fj07xt9t.jpg)

简单的说: CSS、JS加强了HTML

## 二、用相关知识剖析本项目

**项目组成结构**：

  ![](http://ww4.sinaimg.cn/mw690/006rmJyDgw1f3p5hwfzooj30fe0c3dgn.jpg) 

## 三、开始开发

### **(1) 界面篇（HTML、CSS）**

#### **HTML（界面内容）**

- 内容基本思路

 ![](http://ww1.sinaimg.cn/mw690/006rmJyDgw1f3p5hwmgjtj30fe02rmxc.jpg)

- HTML基本结构：

```
<!DOCTYPE html>
<html>
	<head>
		<title>文档标题</title>
	</head>
	<body>
		这里的内容会显示出来
	</body>
</html>
```

首先先把这个元素加入head元素内：

`<metacharset="GBK" />`

设置文本编码，防止乱码（保存文件的编码和charset的值相同）

`<metaname="viewport" content="width=device-width,initial-scale=1.0" />`

配置视图的宽度和比例的，如果未添加，则在手机看要手动放大屏幕(不爽)

分析、编写内容

 ![](http://ww4.sinaimg.cn/mw690/006rmJyDgw1f3p5hxff64j30bh044744.jpg)

分析：这就是一行字而已

- h1:一级标题，默认情况下是字体最大的标题

`<h1>最大的标题</h1>`

 ![](http://ww3.sinaimg.cn/mw690/006rmJyDgw1f3p5hxnwy0j30bh0f53yx.jpg)

分析：这就是一个表格而已

- table:表格

**简略的table结构**

 ![](http://ww2.sinaimg.cn/mw690/006rmJyDgw1f3p5hypdbnj30g20dnmxe.jpg)

- table：定义一个表格
- tr：表格中的一行
- td：一行中的一个格子

![](http://ww3.sinaimg.cn/mw690/006rmJyDgw1f3p5hz6tlpj30mf05mmxc.jpg)

```
<table>
	<tr>
		<td>AC</td>
		<td>+/-</td>
		<td>%</td>
		<td>/</td>
	</tr>
	<tr>
		<td>7</td>
		<td>8</td>
		<td>9</td>
		<td>*</td>
	</tr>
	<tr>
		<td>4</td>
		<td>5</td>
		<td>6</td>
		<td>-</td>
	</tr>
	<tr>
		<td>1</td>
		<td>2</td>
		<td>3</td>
		<td>+</td>
	</tr>
	<tr>
		<td>0</td>
		<td>.</td>
		<td>=</td>
	</tr>
</table>
```



完成后可在`<table>`加上边框`border="1"`看看效果(既：`<table border= "1 ">`)

HTML页面内容基本完成 !

#### **CSS（界面外观）**

外观基本思路：

![](http://ww4.sinaimg.cn/mw690/006rmJyDgw1f3p5i0nhmxj30ff02qglw.jpg)

在HTML内应用CSS（让CSS能够作用在HTML上）

`<link rel="stylesheet" href="style.css" />`

接下来在有需要的位置加上id和class属性（为了让CSS和JS更方便的操作HTML）

`<td id="ac" class = "gray">AC</td>`

选择器：筛选出其中一些HTML元素，并修饰其外观

在这里我们用到：

![](http://ww3.sinaimg.cn/mw690/006rmJyDgw1f3p5i14c28j30fn06e75b.jpg)

**修饰显示器**：

```
/*显示器的id选择器*/
#text{
	background-color:black;		/*背景*/
	color:white;			/*字体颜色*/
	width:265px;			/*宽度*/
	line-height:2.5;		/*行高*/
	text-align:right;		/*水平对齐方式*/
	padding:10px;			/*内边距*/
	margin:0;			/*外边距*/
}
```



- background-color：背景颜色
- color：字体颜色
- width：宽度
- line-height：行高（字体大小的倍数，值是2.5，则其高度为16*2.5=40px）
- text-align：内容对齐（right左对齐、center居中、right右对齐）
- padding：内边距
- margin：外边距

![](http://ww3.sinaimg.cn/mw690/006rmJyDgw1f3p5i2bv9ij306o06sjrn.jpg)

**修饰按钮**：

```
/*表格一个格子(按钮)的tag选择器*/	
td{
	border:1px solid black;		/*边框(solid代表实线)*/
	width:70px;			/*宽度*/
	height:70px;			/*高度*/
	padding:0;			/*内边距*/
	text-align:center;		/*水平对齐方式*/
}
```



- border：边框（1px边框大小为1px，solid边框是实线，black边框是黑色）
- height：高度

**修饰表格**：

```
/*表格的tag选择器*/
table{
	border-collapse:collapse;	/*边框重叠*/
	width:285px;			/*宽度*/
}
```



- border-collapse：表格边框是否合并，默认是分开（separate），设置为collapse将其合并



**再次修饰按钮**

 ```
/*要设置为白色的按钮的class选择器*/	
.white{
	background-color:white;		/*背景*/
}

/*要设置为灰色的按钮的class选择器*/
.gray{
	background-color:lightgray;	/*背景*/
}

/*要设置为橙色的按钮的class选择器*/
.orange{
	color:white;			/*字体颜色*/
	background-color:darkorange;	/*背景*/
}
 ```

界面搞定!

### **(2) 功能实现**

请查看文件夹中的calculate.js

----------------

><span style="font-size:12px">本文标题: <a href="{{ permalink }}">{{ title }}</a>
文章作者: <a href="http://itxiehui.github.io/">杨世彬</a>  
许可协议: <img alt="知识共享许可协议" style="border-width:0" src="https://i.creativecommons.org/l/by-nc-sa/4.0/80x15.png" /><a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/">©署名-非商用-相同方式共享 4.0</a></span>