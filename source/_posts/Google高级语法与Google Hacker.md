---
title: Google高级搜索语法和Google Hacker
date: 2016-04-24 2:20:45
tags: [网络,实用,安全,黑客]
---

----

搜索引擎，如百度，google我们都会用，但是你们有没有想过它的另外一种用处呢，想知道它有什么强大的功能吗？

>搜索语法用好了你也可以当一名Hacker

<!--more-->

现如今，各种搜索引擎已经成为了我们搜索各类信息的得力助手

如：

> * 百度
> * google
> * 雅虎
> * 搜狗

当然，除了以上列举的那些，还有其他很多搜索引擎，不过我们生活中比较多还是使用的***百度***或者***google***，所以以下的内容也是围绕这两者展开

-----

## **一、网络机器人和Robot.txt**

### **Robot机器人**

搜索引擎使用起来其实很简单，并不需要很高的技术水平，但大家有没有思考过为什么我们能从浏览器访问到各种各样的网页？

>其实呢，这都是***网络机器人Robot***的功劳，Robot亦称为***网络蜘蛛***，***网络爬虫***，是一种按照一定的规则自动抓取万维网资源的**程序**或者**脚本**，已被广泛应用于互联网领域。
![robot](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQQVE93lCRxdqqIza2_KdEVQQaGgz3Unv0Bl_N-bnfwD4D1nmbxRg)
它会从全球的站长手里抓取抓取Web网页、文档甚至图片、音频、视频等资源，然后我们才能通过搜索引擎攫取我们所需。

但是大家有没有想过，如果任由Robot搜索我们服务器上的网站，那么其中存储用户密码的**数据库**以及比较**隐私**的页面岂不是都暴露在别人的面前了，这肯定不是站长建站的初衷。

那么我们必须向一种办法来限制各类机器人的搜索范围.

### **robots.txt**

其实，站长只需要在网站的目录中编辑一下一个文件的内容，就可以**限制**爬虫的爬取了，它就是**robot.txt**
它是这样的：

它其实只有三个字段，通过组合来达到不同的限制目的，所以并不难：

 >* user-agent：描述搜索引擎robot的名字
 >* allow：允许robot访问的页面
 >* disallow：不允许robot访问的页面
 
robots.txt写法举例：
 
**1、禁止所有搜索引擎访问网站的任何部分：**

```
User-agent: *  
Disallow: /
```

>***注意：*** *是代表所有robot的意思哦，以下不再赘述

**2、允许所有的robot访问**

```
User-agent: *
Disallow:
```

或者也可以建一个空文件robots.txt

>***注意***：disallow后面不加内容跟allow *是相同效果的哦！

**3、禁止所有搜索引擎访问网站的几个部分（下例中的cgi-bin、tmp、private目录）**

```
User-agent: *
Disallow: /cgi-bin/
Disallow: /tmp/
Disallow: /private/
```

**4、禁止某个搜索引擎的访问（下例中的BadBot）**

```
User-agent: BadBot
Disallow: /
```

**5、只允许某个搜索引擎的访问（下例中的WebCrawler）**

```
User-agent: WebCrawler
Disallow:
User-agent: *
Disallow: / `
```

>robots文件写好了能够减少我们以后一些安全隐患哦，所以将成为站长的各位要留意了~~

-----

## **二、翻墙**

![GFW](https://farm4.staticflickr.com/3098/4563269687_a242241929_o.png)
在进入正式课程前，大家还是要先了解下”**翻墙**“哦
我们现在可以上的网站都得经过国家批准反动的或者和某party利益不符合的都会被**长城防火墙（GFW）**pass掉哦~。~。

那么，我们就要采取一种技术来绕过GFW了，这个绕过的过程就是翻墙啦！
这里不讲原理，直接贴软件：**[lantern](https://getlantern.org/)（比较推荐），赛风**等等

那么，介绍完一些相关知识，下面我们就正式进入google高级语法这门课程了

------

## **三、google高级搜索语法**

>友情提示：这些语法不仅面向计算机专业人员，也同样适用于普通用户
掌握这些语法，日常生活中，我们可以通过它来缩小robot的搜索范围，同时也返回更精确的信息，更加符合我们的要求

比如下面用处：

- [x] 搜索pdf文件
- [x] 搜索招聘信息
- [x] 搜索简历模板
- [x] 搜索网站后台
- [x] 搜索数据库下载页面

> 注意：以下内容不分先后，我们主要学会的是灵活组合使用，那样威力会加倍哦！

### **NO.1  filetype**

为什么我要先贴这条语法呢，主要还是因为他对普通用户也具有很强的实用性，能够在茫茫cyber海洋中，将目光锁定到符合我们要求的资源上
> **filetype：pdf** 

![filetype-exa](http://ww4.sinaimg.cn/mw690/006rmJyDgw1f375x4saxlj30f90fh43s.jpg)

这样我们可以指定我们想要的任何文件类型，不用再漫无目的地查找，吃力不讨好，一条简单的命令就过滤了许许多多无用的信息

> 注意：这里虽然此功能很强大，但是支持的文件格式只有13中哦，除了上面的PDF文档，还有一下几种：

- Microsoft Office(doc,ppt,xls,rtf)
- Shockwave Flash(swf)
- PostScript(ps)
- 其他

不过个人感觉，其实这已经能够应付大多时候的需求了^_^

当然还可以加上一些关键字哦，如：**photoshop使用技巧**，效果如下图所示：
![filetype-exa2](http://ww3.sinaimg.cn/mw690/006rmJyDgw1f3761kt2znj311h0k1wm7.jpg)

### **NO.2 趁热打铁--site**

前面指定了photoshop教程，但是来源却并不一样，如何指定来源呢，就比如**百度文库**，我们可以用

> **site : 关键字**，指定页面来源的站点

> **site**后面接的是类似`www.baidu.com`这样的站点，顶级域名后面的东西就不需要了

那么查找百度文库的有关photoshop的pdf文档是这样的：
> **filetype:pdf   site:wenku.baidu.com     photoshop实用教程**

![site-exa-1](http://ww2.sinaimg.cn/mw690/006rmJyDgw1f375u8424kj310k0hc12z.jpg)

### **NO.3 intitle**

> **intitle：页面标题中含有**关键字**

科比已经退役了，可惜我还没听过他的音乐会呢~~
前面讲了site，那么如果我们想找youtube网站中页面标题中含有“科比”的页面，听听他的”*concert*“，又该怎么办呢？
> **site:youtube.com  intitle:Kobe Bean Bryant**
就如下图所示，“concert”有了，让我们一起倾听吧！
![intitle-exa-1](http://ww1.sinaimg.cn/mw690/006rmJyDgw1f37638bm8sj30ht0grn2q.jpg)

### **NO.4 inurl**

>**inurl : 代表网址中含有关键字**

这里给一条比较有趣的命令：
> **inurl : "viewerframe?mode="**

搜索后找个页面点击进去，你会发现很多实时影像，这其实就是没有密码保护的(Panasonic)松下网络摄像头的监控画面，这种监控头遍布各地，所以你能领略国外的风景哦，看看国外的建筑风格，满足下我们的好奇心,嘻嘻！
![inurl-exa](http://ww3.sinaimg.cn/mw690/006rmJyDgw1f376547a3zj30lb0dqtda.jpg)

### **NO5. intext**

>**intext:网页中出现的关键词**

下面举个例子，最近**《太阳的后裔》**挺火的，那么如果我们要在腾讯新闻（new.qq.com）中搜索关于宋仲基欧巴的信息，怎么办呢，

> **site:news.qq.com  intext: 宋仲基**
如下图：
![intext-exa1](http://ww3.sinaimg.cn/mw690/006rmJyDgw1f3766mb2u3j30ie0ef0yi.jpg)
那么你们也可以试试搜索“乔妹”，“金正恩”的信息

google高级语法还有其他的小特性，不过经常会用到的也就这几个了，下面我们会正式介绍这些语法在hacker手中的作用以及google hacker！

----

## **四、Google Hacker**

![hacker](http://ww2.sinaimg.cn/mw690/006rmJyDgw1f3768mtvcdj307n0530so.jpg)
进入下面内容之前，必须重申一下：
我们学习这些内容知识为了更好地了解黑客的攻击手段，从而更好地防护以及做出应对策略，而不是去攻击别人，而且我们用这些语法的时候，google会视为异常流量，会记录ip的，所以别乱搞~

### **What is Hacker？**

黑客：Hacker一词，最初曾指热心于计算机技术、水平高超的电脑专家，尤其是程序设计人员，逐渐区分为白帽、灰帽、黑帽等，其中黑帽（black hat）实际就是cracker。在媒体报道中，黑客一词常指那些软件骇客（software cracker），而与黑客（黑帽子）相对的则是白帽子。

> *黑客是有建设性的，而骇客则专门搞破坏*

hacker的成就需要大量的时间和精力，但是也有另外一种hacker，只靠一个google搜索引擎，可以在几秒之内秒掉一个站.
这并不需要很高深很高深的技术，当然这也是相对而言的，我们只要一步步了解，一步步深入，再有适当工具的搭配，离google hacker并不会多遥远！

下面，就让我们一起学习吧！

> **intitle：hacker用它搜索后台登陆页面**

常见的有：

- **intitle：登录**
- **intitle：管理**

效果如图：
![intitle-exa1](http://ww1.sinaimg.cn/mw690/006rmJyDgw1f375u8taxpj311o0hfjvo.jpg)

你会发现貌似搜索出来的很多都不符合我们的要求，别急，下面有更好的方法
>**inurl：login.asp**
这样我们搜索出来的都是***后台的管理页面***了，看图：
![inurl-exa2](http://ww4.sinaimg.cn/mw690/006rmJyDgw1f375u9awcpj311y0k4454.jpg)

进了后台之后，除了暴力破解，
还可尝试万能密码哦！
附：[安全焦点](http://www.chncto.com/heikejishu/20658.html)
> **如果你有幸成功了，友情测试还是可以的，但是请你不要破环人家的成果**

----

利用上传页面传木马
> inurl：inurl:upload.asp
  inurl：upload_soft.asp

![upload-exa1](http://ww3.sinaimg.cn/mw690/006rmJyDgw1f376ftd920j30mr0cs753.jpg)
但是这里要注意，文件上传限制了格式和大小，一般是gif和png，size一般只有几百k，所以木马需要利用一些`系统漏洞`或者通过`数据库差异备份插入`，这里就不细写，思路：`一般先用上传小马，用工具连接，再传大马`
感兴趣的可以上各大安全论坛学习！

-----

入侵渗透一般都是指定拿站，而不是盲目扫描，所以我们在前面的基础上加一重限定就ok了
> site：pyfc.com  inurl：upload_soft.asp

如图：
![upload-exa-2](http://ww4.sinaimg.cn/mw690/006rmJyDgw1f376jcyfqgj30hi0e8dlg.jpg)
其实也就是依靠语法的灵活组合而已！

-----

最后，介绍一个比较危险的***漏洞***，这是由于管理员的疏忽而造成的，windows的IIS安装了web服务后，默认会关闭上***目录浏览***，但是如果由于某些原因打开的话，后果可是很严重，然而网上这种例子还是时有发生！

### **to parent directory**

由于***目录浏览***的“功能”，现在网页不再以http的形式呈现在我们眼前了，而是以一种目录树的方式呈现了，可怕的是，你服务器上web根目录所有文件都可看到哦，包括数据库，password.txt，以及其他一些隐私的文件。
> **intext:to parent directory**

如下图：
![to-1](http://ww3.sinaimg.cn/mw690/006rmJyDgw1f376ll0qwwj30mu0gxdke.jpg)

### **“index of /” 功能很强大**

此功能更上述的“to parent directory”类似，可以直接进入网站首页下的所有文件和文件夹中,只不过可以选择搜索目录，
**常用语法**：

- index of /admin
- index of /passwd
- index of /password
- "index of /" +passwd
- "index of / secret"
> **附**：**“ ”**表示包括的内容不能是间断的，**'+'**表示后面的内容一定要有

效果如下图：

![index](http://ww3.sinaimg.cn/mw690/006rmJyDgw1f376ll0qwwj30mu0gxdke.jpg)
可惜密码是加密的，不过可以通过一些工具进行破解
![index2](http://ww3.sinaimg.cn/mw690/006rmJyDgw1f375u71hzpj305a0hjtcz.jpg)

google hacker的内容到这里也就差不多，更多的内容要靠大家自己去补充^.^


----------------

><span style="font-size:12px">本文标题: <a href="{{ permalink }}">{{ title }}</a>
文章作者: <a href="http://tgsx.github.io/">林溢彬</a>  
许可协议: <img alt="知识共享许可协议" style="border-width:0" src="https://i.creativecommons.org/l/by-nc-sa/4.0/80x15.png" /><a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/">署名-非商用-相同方式共享 4.0</a></span>