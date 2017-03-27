---
title: 域名系统DNS简介
date: 2016-11-16
tags: [网络,分享会,基础]
---

 DNS域名系统的简单介绍，以及部分域名解析的巧妙用法。以及hosts文件的介绍，这里有能上天的hosts文件（定时更新）: ` https://raw.githubusercontent.com/racaljk/hosts/master/hosts`


<!--more-->
# DNS
DNS（Domain Name System，域名系统），因特网上作为域名和IP地址相互映射的一个分布式数据库，能够使用户更方便的访问互联网，而不用去记住能够被机器直接读取的IP数串。通过主机名，最终得到该主机名对应的IP地址的过程叫做域名解析（或主机名解析）。DNS协议运行在UDP协议之上，使用端口号53。
***
![1.jpg](http://ww1.sinaimg.cn/mw1024/8f6eb021jw1facjvvustmj20zk0k0gnp.jpg)
![2.jpg](http://ww2.sinaimg.cn/mw1024/8f6eb021jw1facjvvzykuj20zk0k077l.jpg)
![3.jpg](http://ww2.sinaimg.cn/mw1024/8f6eb021jw1facjvwb39wj20zk0k00vp.jpg)
![4.jpg](http://ww1.sinaimg.cn/mw1024/8f6eb021jw1facjvwh6fnj20zk0k040i.jpg)
![5.jpg](http://ww4.sinaimg.cn/mw1024/8f6eb021jw1facjvwwdqsj20zk0k079f.jpg)
![6.jpg](http://ww2.sinaimg.cn/mw1024/8f6eb021jw1facjvx03rzj20zk0k0tav.jpg)
![7.jpg](http://ww1.sinaimg.cn/mw1024/8f6eb021jw1facjvx69d4j20zk0k0gnj.jpg)
![8.jpg](http://ww1.sinaimg.cn/mw1024/8f6eb021jw1facjvxe702j20zk0k0q5l.jpg)
![9.jpg](http://ww2.sinaimg.cn/mw1024/8f6eb021jw1facjvxovtej20zk0k040u.jpg)

通过这个hosts，我们就可以上那些我们“无法连接”的网站。

------

> <span style="font-size:12px">本文标题: <a href="{{ permalink }}">{{ title }}</a>
> 文章作者: <a href="http://itxiehui.github.io/">严浩林</a>  
> 许可协议: <img alt="知识共享许可协议" style="border-width:0" src="https://i.creativecommons.org/l/by-nc-sa/4.0/80x15.png" /><a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/">©署名-非商用-相同方式共享 4.0</a></span>
