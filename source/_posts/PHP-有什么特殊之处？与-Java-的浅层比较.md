---
title: PHP 有什么特殊之处？与 Java 的浅层比较
date: 2016-04-03 09:27:52
tags: [编程,Java,PHP]
---

PHP中的变量以`$`符号开头，如果是全局变量则是以`$_`开头并且都大写，如`$_REQUEST`。在Java中变量可以是字母(a-z)、下划线(-)或者美元符号($)开头，并且变量必须先有类型声明，如`int i = 0`，随后才能使用。但是PHP却不用，它的类型完全由赋值确定。你给它什么，它就是什么。那为什么要强制使用美元符开头呢？

<!--more-->

记得在PHP中有这样的输出语句 `echo "$name is me."`，假如没有美元符，PHP又如何识别`$name`是一个变量呢？

### 讲到输出语句，就不得单双引号在PHP中区别。

在Java中，我们使用输出语句通常是`System.out.println();`括号里可以是任意基本类型+String。而在PHP中输出语句更丰富，有echo，print，print_r还有var_dump。以echo为例，假定`$name = 'Lshare';`。对于`echo '$name is me.'`，PHP将不会对单引号里面的字符串进行解析，而是直接输出`$name is me.`；对于`echo "$name is me."`，PHP对双引号里的字符串进行解析，输出`Lshare is me.`。双引号的代价是效率上比单引号差，但可读性好。为了提高性能，你可能这样使用 `echo $name.' is me.'`或` echo $name,' is me.'`，明显可读性会差点，但是性能上比用双引号好，因为不用去解析引号内的字符串了。

有趣的是`echo`和`print`他们并不是函数，而是语言结构，就好像`if`、`else`那样。你既可以使用`()`来输出单个参数，也可以使用`''`或`""`输出多个参数，中间用`,`隔开，如上文中的` echo $name,' is me.'`。这就是它的语法，就好像你学习`if...else...`一样，懂了这些才可以随心所欲而不逾矩。

初学者可能会对这样的语句感到疑惑`echo '<br/>'`，认为浏览器应该直接输出`<br/>`，因为单引号内，PHP是不会进行解析的。哈，PHP是不解析，但是浏览器解析啊。回想一下，浏览器不就是接收Html类型的数据，然后解析呈现给用户吗？PHP把`<br/>`给了浏览器，然后浏览器解析成换行，难道有什么不对吗？

### 再谈谈数组。

在Java中我们通过`int[] arr = new int[]{1,2,3};`来创建一个数组，而在PHP中确实用`array()`函数来创建的。而且PHP中的数组还有键值对的概念，这不是跟Java中的Map很像吗？你可以不指定键，它会默认从0开始作为索引键；你也可以使用`=>`来指定键。如下：

```
$arr = array( 
    "1"=>"Google",
    "3"=>"Microsoft",
    "2"=>"Apple"
);
```

###  说到数组就不得不谈谈遍历

遍历可以使用for循环，也可以使用foreach循环。for循环跟Java中的差不多，不谈，还是谈谈foreach循环有趣。在Java中的foreach循环是这样的：

```
int[] arr = new int[]{1,2,3};
for(int item:arr){//":"前面是数组中元素的类型和临时变量，后面的是数组
    System.out.println(item);
}
```

而在PHP中是这样的：

```
$arr = array(
    1,2,3
);
//第一种
foreach($arr as $value){//as前面是数组，后面是数组中元素的临时变量。
    echo "$value<br/>";
}
//第二种
foreach($arr as $key=>$value){//同上，不过同时保存了键值。
    echo "$key = $value<br/>";
}
```

简洁了好多有木有？网络时代讲究的简洁高效，不浪费带宽，PHP就是这样。


### 谈谈字符串和数组的转化

在Java中将字符串切割成数组，最简单的是这样：

```
String str = "1-2-3";
String[] arr = str.splite("-");
```

而在PHP中提供了`explode`函数：

```
$str = "1-2-3"; 
$arr = explode("-",$str);
```

在Java中将数组转成字符串

```
String str2 = Arrays.toString(arr);//是"[1, 2, 3]"
//或者要自定输出格式的话
StringBuilder sb = new StringBuilder();
for(int i=0;i<arr.length;i++){
	sb.append(arr[i]);
	if(i!=arr.length-1){
		sb.append("-");
	}
}
String str3 = sb.toString();//是"1-2-3"
```

而在PHP中没那么麻烦，直接用`implode`函数就够了。

```
$str2 = '['.implode(",",$arr).']'; //是"[1, 2, 3]"
$str3 = implode("-",$arr); //是"1-2-3"
```

一切变得如此简洁，世界一下子就好了。

### 说到数组，还真的避不了数组排序

在Java中提供了`Arrays.sort()`的许多重载方法，用于数组的排序。

```
int[] ranks = new int[]{2,9,1,5,7};
//使用默认的比较器，进行升序排序
Arrays.sort(ranks); //为 "[1, 2, 5, 7, 9]"
//自定义比较器，进行降序排序
Integer[] ranks = new Integer[]{2,9,1,5,7};
Arrays.sort(ranks, new Comparator<Integer>() {
	@Override
	public int compare(Integer o1, Integer o2) {
		return o2-o1;
	}
});// 为 "[9, 7, 5, 2, 1]"
```

在PHP中没那么麻烦，当然定制性没那么好。

```
$ranks = array(
    2,9,1,5,7
);
sort($ranks); //按值升序排序
rsort($ranks); //r表示reverse，相反的意思。按值降序排序
asort($ranks); //a表示association，键值联系的意思。保持键值关系升序排序
arsort(); //保持键值关系降序排序
ksort($ranks); //k表示key，按键升序排序
krsort(); //按键降序排序
```

### 再说一点表单的

当使用表单提交多选的值时，要注意name属性的值后面要带"[]"，不然PHP中接收的是单个值而不是一个数组。如：

client.html
```
<form action="server.php" method= "POST">
    <input type="checkbox" name="city[]" value="gz" />广州
    <input type="checkbox" name="city[]" value="bj"/>北京
    <input type="checkbox" name="city[]" value="sz"/>深圳
    <input name="submit" type = "submit" value="提交"/>
</form>
```
server.php

```
<?php
if(isset($_POST['city'])){ //判断变量是否有值
    $citys = $_POST['city'];
    echo '['.implode(',',$citys).']'; 
}
?>
```

----------------

><span style="font-size:12px">本文标题: <a href="{{ permalink }}">{{ title }}</a>
文章作者: <a href="http://itxiehui.github.io/">IT协会知识库</a>  
发布时间: {{ date }}
最后更新: {{ updated }}
许可协议: <img alt="知识共享许可协议" style="border-width:0" src="https://i.creativecommons.org/l/by-nc-sa/4.0/80x15.png" /><a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/">©署名-非商用-相同方式共享 4.0</a></span>