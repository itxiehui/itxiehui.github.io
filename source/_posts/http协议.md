---
title: http协议
date: 2016-05-09 15:45:24
tags: [编程,协议]
---

生活中，我们为了获取我们想要的信息资源而通过浏览器browser浏览一个个的网址，那么这其中的实现机制是怎么样的呢？
其实，浏览器/服务器(B/S)架构是基于http协议的，所以学习这个协议对于我们理解基于它的上层服务会很有帮助。
举例，在用python编写网络爬虫的时候就会涉及到操作响应码，cookie，session，所以学习http是很有必要的！！！

<!--more-->

## 什么是HTTP协议

　　协议是指计算机通信网络中两台计算机之间进行通信所必须共同遵守的`规定或规则`，超文本传输协议(HTTP)是一种通信协议，它允许将超文本标记语言(HTML)文档从Web服务器传送到客户端的浏览器

     　目前我们使用的是HTTP/1.1 版本
------

## Web服务器，浏览器，代理服务器

　　当我们打开浏览器，在地址栏中输入URL，然后我们就看到了网页。 原理是怎样的呢？

　　实际上我们输入URL后，我们的浏览器给Web服务器发送了一个**Request**, Web服务器接到Request后进行处理，生成相应的Response，然后发送给浏览器， 浏览器解析**Response**中的HTML,这样我们就看到了网页，过程如下图所示

![photo](http://ww4.sinaimg.cn/mw690/006rmJyDgw1f3p5bh4i7dj30l209tjuc.jpg)

　　我们的Request 有可能是经过了代理服务器，最后才到达Web服务器的。

　　过程如下图所示

![photo2](http://ww1.sinaimg.cn/mw690/006rmJyDgw1f3p5bhoxd4j30m109p41w.jpg)

　　代理服务器就是网络信息的中转站，有什么功能呢？

　　1. 提高访问速度， 大多数的代理服务器都有缓存功能。

　　2. 突破限制， 也就是翻墙了

　　3. 隐藏身份。

-------

## URL详解
其实定位一个资源最重要的就是URI了，而URL和URN是URL的两个子集。
通常，我们习惯直接用URL来描述URI。

    URI ：Uniform Resource Identifier，统一资源标识符；
    URL：Uniform Resource Locator，统一资源定位符；
    URN：Uniform Resource Name，统一资源名称。
　　URL(Uniform Resource Locator) 地址用于描述一个网络上的资源，  基本格式如下

    schema://host[:port#]/path/.../[;url-params][?query-string][#anchor]

　　scheme               指定低层使用的协议(例如：http, https, ftp)

　　host                   HTTP服务器的IP地址或者域名

　　port#                 HTTP服务器的默认端口是80，这种情况下端口号可以省略。如果使用了别的端口，必须指明，例如 http://www.cnblogs.com:8080/

　　path                   访问资源的路径

　　url-params

　　query-string       发送给http服务器的数据

　　anchor-             锚

###　URL 的一个例子

    http://www.mywebsite.com/sj/test;id=8079?name=sviergn&x=true#stuff

    Schema: http

    host: www.mywebsite.com

    path: /sj/test

    URL params: id=8079

    Query String: name=sviergn&x=true

    Anchor: stuff
---

## HTTP协议是无状态的

　　http协议是`无状态`的，同一个客户端的这次请求和上次请求是没有对应关系，对http服务器来说，它并不知道这两个请求来自同一个客户端。 为了解决这个问题， Web程序引入了`Cookie`机制来维护状态.
## HTTP消息的结构

　　先看`Request`消息的结构，Request消息分为3部分，第一部分叫请求行， 第二部分叫http header, 第三部分是body. header和body之间有个空行， 结构如下图

![photo3](http://ww3.sinaimg.cn/mw690/006rmJyDgw1f3p5bhx4jkj30by05e3ye.jpg)

　　第一行中的Method表示请求方法，比如"POST"，"GET"，  Path-to-resoure表示请求的资源， Http/version-number 表示HTTP协议的版本号

　　当使用的是"GET" 方法的时候， body是为空的

　　比如我们打开博客园首页的request 如下

    GET http://www.cnblogs.com/ HTTP/1.1

    Host: www.cnblogs.com

　　我们用Fiddler 捕捉一个博客园登录的Request 然后分析下它的结构， 在Inspectors tab下以Raw的方式可以看到完整的Request的消息，   如下图

![photo4](http://ww1.sinaimg.cn/mw690/006rmJyDgw1f3p5bibqmhj30qu0lk3zl.jpg)
　　
　　我们再看`Response`消息的结构， 和Request消息的结构基本一样。 同样也分为三部分，第一部分叫request line, 第二部分叫request header，第三部分是body. `header和body之间也有个空行`，  结构如下图

![photo6](http://ww2.sinaimg.cn/mw690/006rmJyDgw1f3p5bj9aeij30by05ejr9.jpg)

　　HTTP/version-number表示HTTP协议的版本号，  status-code 和message 请看下节[状态代码]的详细解释.

　　我们用Fiddler 捕捉一个博客园首页的Response然后分析下它的结构， 在Inspectors tab下以Raw的方式可以看到完整的Response的消息，   如下图

![photo4](http://ww3.sinaimg.cn/mw690/006rmJyDgw1f3p5bjmfbvj30rm0kowft.jpg)

----

## Get和Post方法的区别

　　Http协议定义了很多与服务器交互的方法，最基本的有4种，分别是GET,POST,PUT,DELETE. 一个URL地址用于描述一个网络上的资源，而HTTP中的GET, POST, PUT, DELETE就对应着对这个资源的查，改，增，删4个操作。 我们最常见的就是GET和POST了。GET一般用于获取/查询资源信息，而POST一般用于更新资源信息.

　　我们看看GET和POST的区别

　　1. GET提交的数据会放在URL之后，以?分割URL和传输数据，参数之间以&相连，如EditPosts.aspx?name=test1&id=123456.  POST方法是把提交的数据放在HTTP包的Body中.

　　2. GET提交的数据大小有限制（因为浏览器对URL的长度有限制），而POST方法提交的数据没有限制.

　　3. GET方式需要使用Request.QueryString来取得变量的值，而POST方式通过Request.Form来获取变量的值。

　　4. GET方式提交数据，会带来安全问题，比如一个登录页面，通过GET方式提交数据时，用户名和密码将出现在URL上，如果页面可以被缓存或者其他人可以访问这台机器，就可以从历史记录获得该用户的账号和密码.




----------------

><span style="font-size:12px">本文标题: <a href="{{ permalink }}">{{ title }}</a>
>文章作者: <a href="http://tgsx.github.io/">林溢彬</a>  
>许可协议: <img alt="知识共享许可协议" style="border-width:0" src="https://i.creativecommons.org/l/by-nc-sa/4.0/80x15.png" /><a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/">?署名-非商用-相同方式共享 4.0</a></span>