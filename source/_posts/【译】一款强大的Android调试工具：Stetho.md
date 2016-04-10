---
title: 【译】一款强大的Android调试工具：Stetho
date: 2016-03-30 14:25:24
tags: [编程,Android]
---

本文翻译自：[http://facebook.github.io/stetho/](http://facebook.github.io/stetho/)。没错，Facebook出品，必属精品！

> 一个专门给Android应用使用的调试桥工具

Stetho是一个给Android应用使用的高级调试桥工具。当启用后，开发者可以使用Chrome桌面浏览器的Chrome开发者工具特性。开发者也可以选择启用可选的dumpapp工具，它提供了一个强大的触及应用程序内部的命令行界面。
<!--more-->
## 下载

[Download v1.3.1](https://github.com/facebook/stetho/releases/download/v1.3.1/stetho-1.3.1-fatjar.jar)

或者你也可以通过Gradle或者Maven，从Maven中心仓库中下载Stetho到你的项目中。

```java
// Gradle
dependencies { 
    compile 'com.facebook.stetho:stetho:1.3.1' 
} 
```

```xml
<!--Maven-->
  <dependency>
    <groupid>com.facebook.stetho</groupid> 
    <artifactid>stetho</artifactid> 
    <version>1.3.1</version> 
  </dependency> 
```

只有stetho核心是必须的，你也可以同时包含进网络库，如：

```java
  dependencies { 
    compile 'com.facebook.stetho:stetho-okhttp3:1.3.1' 
  } 
```

或者

```java
  dependencies { 
    compile 'com.facebook.stetho:stetho-okhttp:1.3.1' 
  } 
```

或者

```java
  dependencies { 
    compile 'com.facebook.stetho:stetho-urlconnection:1.3.1' 
  } 
```

## 特性

#### Chrome开发者工具

![inspector-discovery](http://7xsf09.com1.z0.glb.clouddn.com/16-3-30/15721059.jpg)

Stetho软件通过使用客户端/服务器协议实现了在Chrome开发者工具前端的集成。一旦你的应用集成了Stetho，只要在地址栏上输入`chrome://inspect`并点击`Inspect`就可以开始玩了。

#### 网络监视

![inspector-network](http://7xsf09.com1.z0.glb.clouddn.com/16-3-30/66293422.jpg)

可以使用Chrome开发者工具的全套功能来进行网络监视，包括图片预览、JSON响应协助，甚至是输出HAR格式的追踪信息。

#### 数据库监视

![inspector-sqlite](http://7xsf09.com1.z0.glb.clouddn.com/16-3-30/47307653.jpg)
SQLite数据库可以被可视化和交互式地浏览，并且具备完全的读写能力。

#### 视图层级

![inspector-elements](http://7xsf09.com1.z0.glb.clouddn.com/16-3-30/27240811.jpg)

视图层级功能支持ICS（API 15）以及更高版本的Android系统。有很多很棒的功能，比如说View实例被可视化地放置在视图层级中、视图高亮和点击view就可以跳到它在层级中的位置。


#### Dumpapp

![dumpapp-prefs](http://7xsf09.com1.z0.glb.clouddn.com/16-3-30/13927583.jpg)

Dumpapp扩展超出了Chrome开发者工具所能显示的，它给应用组件提供了一个更具扩展性的命令行界面。它提供了一套默认的插件集，但是dumpapp真正强大之处是你可以创建你自己的插件。

#### JavaScript控制台

![inspector-js](http://7xsf09.com1.z0.glb.clouddn.com/16-3-30/43030945.jpg)

JavaScript控制台允许你执行JavaScript代码，来跟应用甚至是Android SDK交互。

## 集成

#### 设置

集成Stetho对现在的大多数Android应用来说，是无缝和直接的。下面的例子展示了在你的Application中初始化Stetho。

```java
//自定义Application并在清单文件的<application>的name属性中配置
public class MyApplication extends Application {
  public void onCreate() {
    super.onCreate();
    //使用默认配置初始化Stetho
    Stetho.initializeWithDefaults(this);
  }
}
```
这样子会启用许多默认的设置，但是没有启用一些额外的钩子功能（hook）（特别是网络监视）。

接着看下面如何在自己的子系统中指定详细的设置。

#### 启用网络监视

如果你使用流行的2.2.x+或者3.x发布版的OkHttp库，你可以使用监视器系统来自动hook进已存在的栈。目前启用网络监视最简单的最直接的方法是：

```java
OkHttpClient client = new OkHttpClient();
client.networkInterceptors().add(new StethoInterceptor());
```
或者

```java
new OkHttpClient.Builder()
    .addNetworkInterceptor(new StethoInterceptor())
    .build();
```

如果你使用`HttpURLConnection`，那么你可以使用`StethoURLConnectionManager`来帮助你进行网络监视，虽然用这个方法时要留意下一些警告信息。值得一提的是，你必须显示添加` Accept-Encoding: gzip`到请求头中，并手动处理压缩后的响应，以便Stetho报告压缩载荷大小。

查看[stetho-sample](https://github.com/facebook/stetho/tree/master/stetho-sample)项目来获取更多信息。

#### 定制dumpapp插件

定制插件是一个继承dumpapp系统的绝好方法，它还可以在配置中很方便的。简单地替换下你的配置，如：

```java
Stetho.initialize(Stetho.newInitializerBuilder(context)
    .enableDumpapp(new DumperPluginsProvider() {
      @Override
      public Iterable<DumperPlugin> get() {
        return new Stetho.DefaultDumperPluginsBuilder(context)
            .provide(new MyDumperPlugin())
            .finish();
      }
    })
    .enableWebKitInspector(Stetho.defaultInspectorModulesProvider(context))
    .build());
```

查看[stetho-sample](https://github.com/facebook/stetho/tree/master/stetho-sample)项目来获取更多信息。

![PS：倒骑驴的农民公众号开通啦](http://7xsf09.com1.z0.glb.clouddn.com/16-3-30/96983497.jpg)

----------------

><span style="font-size:12px">本文标题: <a href="{{ permalink }}">{{ title }}</a>
文章作者: <a href="http://itxiehui.github.io/">IT协会知识库</a>  
许可协议: <img alt="知识共享许可协议" style="border-width:0" src="https://i.creativecommons.org/l/by-nc-sa/4.0/80x15.png" /><a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/">©署名-非商用-相同方式共享 4.0</a></span>