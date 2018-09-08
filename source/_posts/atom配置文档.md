---
title: windows环境下配置简单实用的atom
date: 2018-09-08 19:07:28
tags: [基础,实用]
---

windows环境下配置简单实用的atom

<!--more-->

# windows环境下配置简单实用的atom

## 安装atom
1. 到atom的官网下载，这里附上[atom官网](http://atom.io/)
2. 点开安装，自动开始安装，安装在默认路径 C:\Users\kratos\AppData\Local\atom
3. 接下来就是等待
![ ](https://wx2.sinaimg.cn/mw690/007d7DTvly1fv2cclsif3j30b40b4dhx.jpg)

## 配置基本atom
~~如果不配置，则可以将atom仅仅当作一个功能很强大的文本编辑器使用~~
##### 先学会如何安装插件，atom的核心就是各种插件。下面以装汉化包为例。
file -> settings

![1](https://wx3.sinaimg.cn/mw690/007d7DTvly1fv2b157we6j30g9061myq.jpg)

>点install，然后在输入栏输入你要的插件的名字，一般是英文，因为一般汉化包的名字都有个Chinese，所以输入Chinese，可以点击package搜索，也可以直接回车，我们选择第一个simplified-chinese-menu，因为这个汉化最彻底最完全（插件自己的界面没有汉化别想了），点蓝色的install，等待一会，当出现Uninstall（卸载）说明已经安装成功了，disable是禁用的意思，enable是启用的意思。


![setting](https://wx1.sinaimg.cn/mw690/007d7DTvly1fv2b1mqpvdj30lx0cl75e.jpg)

>安装好后就是中文菜单了

![拓展](https://wx1.sinaimg.cn/mw690/007d7DTvly1fv2b1r2lbzj30m80j70uo.jpg)

>拓展那里就是你已经安装了的插件（拓展），主题就是atom的皮肤，分为语法主题和外观主题，外观主题是外表看到的界面，语法主题是指你在写代码的时候单词的颜色，不同的语法不同颜色，好的主题可以让你直观看到你代码的结构。

## atom入门配置
#### 使用Atom打造无懈可击的Markdown编辑器
~~这个标题是我偷别人的hhhhhhhhh~~

附上原地址[使用Atom打造无懈可击的Markdown编辑器](https://www.cnblogs.com/libin-1/p/6638165.html)

1.预览增强插件(markdown-preview-plus)

    这个插件比atom自带的markdown-preview更好看，
    但是因为markdown-preview-plus这个插件和atom自带的markdown-preview冲突，
    如果要用前者，先禁用掉后者

2.同步滚动(markdown-scroll-sync)

    这个插件基于markdown-preview，装上重启atom即可使用，
    但是！但是会和上面那个预览增强插件冲突(我搞不懂为啥就不能同时兼容)，
    所以如果想用这个插件，先把上面的卸载掉或者禁用掉，把atom自带的打开，
    然后就可以使用了，这个插件的作用在于你边写markdown文件的时候你如果滚动markdown文件，
    预览的文件也一起滚动，而且还可以跟踪光标，个人觉得在写md的时候这个插件非常实用。

3.代码增强(language-markdown)

    这个插件能给代码着色（虽说本来atom就有，但是有这个更好看），
    还提供了快捷的代码片段生成等功能。

4.图片粘贴(markdown-image-paste)

    这个可能是atom里写markdown最实用的工具了，在别的地方复制一张图片，
    到atom里直接长贴就可以了，它会把这一行的字当作图片的名字，这点稍微注意下就好了，
    它会把粘贴的图片放在和markdown文件同一个文件夹

5.表格编辑(markdown-table-editor)

    写过markdown的人都知道，弄一个表格可以让你哭出来，这个插件就是用来解决这个问题的，
    安装好了之后，你和平时一样，输入 |+空格 然后tab一下，你会发现一个新世界，
    换行直接回车即可，自动替你排版。

## C/C++编译环境
>我是从这里学的，把原地址放上[Atom安装并配置C/C++开发环境](http://blog.csdn.net/qq_36731677/article/details/54609583)

#### MinGW的安装
>在windows上安装gcc/g++并不像Linux上那么简单，所以我从这个博主那里学到了一个方法，用MinGW来安装，[MinGW的官网下载链接](https://sourceforge.net/projects/mingw/files/), 安装的过程就是一直continue就好了

![读条](https://wx4.sinaimg.cn/mw690/007d7DTvly1fv2b240pkxj30fz087jt4.jpg)

>到这里就等待就好了，好了之后也是continue，接着会自动弹出MinGW

![选择](https://wx4.sinaimg.cn/mw690/007d7DTvly1fv2b25vft9j30xo0bo74k.jpg)

>选择basic setup的mingw-gcc-g++点一下，mark for installation。
然后点击左上角的installation，然后apply changes，接下来再apply就可以了，
安装好了之后配置环境变量，右键我的电脑，打开“系统属性”，打开“环境变量” 。
在“系统变量”中选择“Path”并点击“编辑”
点击“新建”，并输入“C:\MinGW\bin”也就是minGW的安装路径，如果改了就改成一样就好了


![cmd](https://wx3.sinaimg.cn/mw690/007d7DTvly1fv2b2b5bwnj30rl0efmxj.jpg)

配置好了之后在cmd输入gcc -v检查版本，如果有输出说明装成功了

#### linter-gcc和linter的安装

如果安装好了gcc，我相信这一步对你来说很简单了，搜索上面这两个插件并安装，下载好了之后


![设置](https://wx1.sinaimg.cn/mw690/007d7DTvly1fv2b2gcrlcj30m80h13zk.jpg)

在这里进入设置


![gcc](https://wx2.sinaimg.cn/mw690/007d7DTvly1fv2b2lpohfj30hk0obdhl.jpg)

设置为gcc/g++ (一般情况下用gcc编译c文件，用g++编译cpp文件，也就是C++。),如果是C语言的话，建议填gcc，

![随时](https://wx4.sinaimg.cn/mw690/007d7DTvly1fv2b2pq9ktj30fr0qtmz3.jpg)

>建议勾上这个，这个勾上后可以一边写一边报错，如果不勾则每一次保存或者编译才会报错，下面的数字代表着多少毫秒更新一次，比如我打了一行代码，100毫秒后会在这行代码的前面有红色的点（警告错误）或者黄色(提醒)，如果不喜欢频繁提醒，可以调高时间(默认300ms)或者关闭。


![左下角](https://wx4.sinaimg.cn/mw690/007d7DTvly1fv2c1f8ggtj30am03nt8m.jpg)

默认linter是在左下角的，点一下就弹出来了，可以拖动到左边，右边，或者下面


![实例1](https://wx1.sinaimg.cn/mw690/007d7DTvly1fv2c1iqcxoj31960a20tv.jpg)
![实例2](https://wx2.sinaimg.cn/mw690/007d7DTvly1fv2c1m97ssj31bp0axwft.jpg)

#### gcc-make-run的安装
>到这里其实已经可以写代码了，但是我还得复制代码去别的地方编译，麻烦，我直接安装这个插件一键调出就好啦
安装就不教了，到这里不可能还不会，直接讲使用，按下F6可以直接编译

![编译](https://wx2.sinaimg.cn/mw690/007d7DTvly1fv2c1t0xwsj30e704smx6.jpg)

弹出绿色说明成功了

![成功](https://wx2.sinaimg.cn/mw690/007d7DTvly1fv2c1ugvzyj31bo0tc0ti.jpg)

但是有些人编译出来发现中文是乱码，怎么办呢，右下角的UTF-8点一下，输入GBK，搞定。但是每次都需要改，好麻烦，那么这里改成默认gbk啊hhhhhhhh

![utf8想杀人](https://wx1.sinaimg.cn/mw690/007d7DTvly1fv2c1vw2wfj30lj0kzgoh.jpg)

现在，你可以在atom随心所欲了。

## 进阶
>相信做了这么多，已经差不多掌握基本的atom操作方法了，那么就需要~~装逼~~方便的工具啦，

1.atom-beautify

    在Atom中美化代码的格式，比如HTML，CSS，JavaScript，PHP，Python，Ruby，Java，
    C，C ++，C＃，Objective-C，CoffeeScript，TypeScript，Coldfusion，
    SQL反正好多都可以，巨神奇

2.atom-html-preview + autocomplete-html-entities

    前者用来渲染html，后者给html更完善的自动补全

3.color-picker

    我相信不仅仅是我不会计算颜色，用这个你就可以直接用CTRL+alt+C选色，
    建议在设置里把RGB改成HEX，atom的rgb有点问题，hex就很正常

4.file-icons

    这是用来显示文件图标的hhhhhhh好看
![图标](https://wx2.sinaimg.cn/mw690/007d7DTvly1fv2c2567a4j308t0l1t95.jpg)

5.git-plus

    用git基本都要装，有这个好用很多，方便，ctrl+shift+P调出直接就可以了，就是要在gitbash声明自己先

	
><span style="font-size:12px">文章标题: <a href="{{permalink}}">{{ title }}</a>
文章作者: <a href="http://itxiehui.github.io/">林成耿</a>  
许可协议: <img src="https://i.creativecommons.org/l/by-nc-sa/4.0/80x15.png" style="border-width: 0;" alt="知识共享许可协议"   />
<a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/">©署名-非商用-相同方式共享 4.0</a></span>