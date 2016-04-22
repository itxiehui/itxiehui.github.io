---
title: Toolbar的使用
date: 2016-04-14 12:19:26
tags: [编程,Android]
---

### 简介

Toolbar是Google推出的用来替代Actionbar的一个控件，Toolbar相比于Actionbar也更加的灵活，开发者可以自定义图标，logo等，接下来就介绍一下Toolbar的用法

<!--more-->
    
### 新增Style样式

```java
<style name="AppTheme" parent="Theme.AppCompat.Light.DarkActionBar">
         Customize your theme here.
        <item name="colorPrimary">@color/colorPrimary</item>
        <item name="colorPrimaryDark">@color/colorPrimaryDark</item>
        <item name="colorAccent">@color/colorAccent</item>
        <item name="android:actionOverflowButtonStyle">@style/overflowButton</item>
</style>
<style name="AppTheme.NoActionBar">
        <item name="windowActionBar">false</item>
        <item name="windowNoTitle">true</item>
</style>
```

这些样式一般在Android Studio是自动生成，可不修改，但是假如要修改overflowButton的图标，就需要在此处修改，在接下来的overflow修改中将会介绍。

### XML布局中新增Toolbar

```java
<android.support.v7.widget.Toolbar android:id="@+id/toolbar"
            android:layout_width="match_parent" 
            android:layout_height="48dp"
            android:background="?attr/colorPrimary" 											app:popupTheme="@style/AppTheme.PopupOverlay" />
```

### 在程序中替代Actionbar

```java
		Toolbar toolbar = (Toolbar) findViewById(R.id.toolbar);
        setSupportActionBar(toolbar);
        //设置返回按钮
        getSupportActionBar().setHomeButtonEnabled(true);
```

上面步骤就可以在你的程序中生成一个Toolbar了，接下来介绍如何在Toolbar中添加菜单。

### 往Toolbar添加菜单

首先找到你工程中的Menu文件夹，在里面，你可以设置你Toolbar的菜单项，如下

```java
<menu xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools" tools:context=".MainActivity">
    <item android:id="@+id/action_search" android:title="Search"
        android:icon="@drawable/actionbar_search_icon"
        app:showAsAction="ifRoom"/>
    <item android:id="@+id/action_settings" android:title="@string/action_settings"
    android:icon="@drawable/ic_settings"  app:showAsAction="never" />
</menu>
```

这里设置showAsAction，如果值是ifRoom，那么你这个子项在Toolbar有空间的时候就会显示在Toolbar中，如果设置为never，则会放置在overflowMenu（溢出菜单）中，假如你Toolbar的显示的子项超过三个，那么其他的子项也会放置在溢出菜单中。

当出现了溢出菜单，那么就有一个overflowButton出现了，它呈现出的是三个点，按下是，就会显示出溢出菜单，假如我们想修改这个图标，那么我们应该如何修改，我们可以像下面这样做

```java
<style name="overflowButton">
        <item name="android:src">@drawable/actionbar_add_icon</item>
</style>
```

在style.xml文件中新增如上的样式，同时将这个style作为子项，新增到开头那个style根节点中。这样，你便可以修改这个overflowButton的图标了。

接下来继续将菜单的问题，当按上面那样做了，是出现了菜单，但是你有没有发现，我们在setting那个子项设置了icon却没有显示出来。我们要让图标显示，就要用到反射技术，我们在MainActivity中重写onMenuOpened方法，如下

```java
  @Override
    public boolean onMenuOpened(int featureId, Menu menu) {
        if (menu!=null){
            if (menu.getClass().getSimpleName().equals("MenuBuilder")){
                try {
                    Method method = menu.getClass().getDeclaredMethod("setOptionalIconsVisible",Boolean.TYPE);
                    method.setAccessible(true);
                    method.invoke(menu,true);
                } catch (NoSuchMethodException e) {
                    e.printStackTrace();
                } catch (InvocationTargetException e) {
                    e.printStackTrace();
                } catch (IllegalAccessException e) {
                    e.printStackTrace();
                }
            }
        }
        return super.onMenuOpened(featureId, menu);
    }
}
```

这里有个坑，在第一个if语句的判断条件这里，网上大部分人都是写

```java
 if (featureId== Window.FEATURE_ACTION_BAR && menu!=null)
```

但是当我也按这样的方式写的时候，图标却是显示不出来的，把那个featureid判断的去掉便可以了，所以这里也提醒一下，这个问题也折磨了我好久，一直Google，直到在CSDN上看到一篇博客才解决了这个问题。先写到这里，等到有更多的关于Toolbar的心得再继续写。

----------------

><span style="font-size:12px">本文标题: <a href="{{ permalink }}">{{ title }}</a>
文章作者: <a href="http://itxiehui.github.io/">陈泽佳</a>  
许可协议: <img alt="知识共享许可协议" style="border-width:0" src="https://i.creativecommons.org/l/by-nc-sa/4.0/80x15.png" /><a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/">©署名-非商用-相同方式共享 4.0</a></span>
