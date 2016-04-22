---
title: '#Android源码#Android Touch事件分发机制思考'
date: 2016-04-22 11:04:14
tags:[编程,Android,事件分发]
---

-------- 写在前面 ------------------------------------------------

用一天来思考Touch事件的分发，值了！参考了一些知名的博客，由于并没有引用其中的原文就不具体写引用了。Google一下就有了。以下是我的看法，有错漏的欢迎指正。

<!--more-->

##一、概述

Android中的事件分发是遵循类似责任链模式的，就是从根节点开始逐层往里分发事件，直到找到责任人（即响应事件的View）或找不到责任人事件“丢弃”为止。

##二、什么是Touch事件？

通过屏幕交互的事件，比如单击、长按、滚动等等归根结底都是TouchEvent（触摸事件）。在触屏时代，人与屏幕交互不都是触摸事件吗？

一个完整触摸事件包括**按下**（1个）、**移动**（0-n个）和**抬起**（1个）三个事件组成。所以，当你在屏幕上点击一下，实际上是触发了两个事件，一个是按下，一个是抬起。也有一种事件是**取消**，比如当你按下了按钮，然后移动到别处（按钮区域外）抬起，就会识别为事件取消。

##三、对Touch事件的基本认知

* 一个事件只能被消费（consume）一次，事件消费了就不再传递。
* 一个事件只能有一个责任人。
* 事件以按下为起点，谁消费了按下事件，后续的事件就交给谁处理。
* View可以自己处理事件，也可以分发给子View处理。

##四、谁来处理Touch事件？

这个问题简单，就是View。看官方对View类的功能定义就知道

```
A View occupies a rectangular area on the screen and is responsible for drawing and event handling.
//翻译：
//View在屏幕上占据一块矩形区域，
//负责绘图和事件处理。
```

这样就完了好像有点扫兴。那就再说点吧！

-------华丽的分割线-------------------------------------------------

先阐明一个简单的道理：**ViewGroup是View的子类**，继承有View的非私有方法。普通的View无法包裹子布局，而ViewGroup可以包裹子布局，用脑图简单表示下

![这里写图片描述](http://img.blog.csdn.net/20160130161118833)


再卖个关子，在继续往下读之前，不妨新建一个HelloWord项目，然后用`\sdk\tools\hierarchyviewer.bat`查看MainActivity的视图层级。是这样的

![这里写图片描述](http://img.blog.csdn.net/20160130153726804)

删繁就简，成这样了

![这里写图片描述](http://img.blog.csdn.net/20160130154658774)

简单描述下就是，Activity中持有一个叫PhoneWindow的窗口用来与外部交互，而PhoneWindow持有一个DecorView用来显示可交互的界面，而DecorView持有一个子布局LinearLayout，LinearLayout持有两个子布局，一个是ActionBarContainer（当请求无标题`requestWindowFeature(Window.FEATURE_NO_TITLE)`时为ViewStub），另一个是FrameLayout。FrameLayout的子布局就是我们使用`setContentView(int resId)`挂载的布局啦！！！

根据上面的道理：只有ViewGroup可以包裹子布局。可以得出：**DecorView、ActionBarContainer都是ViewGroup的子类，因为ViewGroup是View的子类，自然也有绘图和处理事件的职责。**

注：关于PhoneWindow和DecorView的源代码可以到http://androidxref.com/这里去搜索。

那么问题来了，Touch事件在这种层级式的视图中是怎么被处理的？？

##五、怎么处理Touch事件？

有两种方案：
1. 从子布局到父布局，即事件先传递给子布局，然后再传递给父布局
2. 从根布局到子布局，即事件先传递给根布局，然后再传递给子布局

按照人的习惯思维，似乎第一种方案更好，所谓“近水楼台先得月”嘛。那第二种我们不也可以理解为“岂有儿子比老子先拿之礼”，老子不要了才给儿子。还有如果用第一种方案的话，老子就不能拦截儿子的事件了，而儿子可以瞒着不告诉老子。

-------华丽的分割线-------------------------------------------------

事实上，Android在设计事件处理时是采用第二种的。

## 六、View中对事件的处理

在View中定义了跟事件处理相关的两个重要函数
一个是`dispatchTouchEvent`方法，另一个是`onTouchEvent`方法，源代码如下：

```
   /**
     * Pass the touch screen motion event down to the target view, or this
     * view if it is the target.
     *
     * @param event The motion event to be dispatched.
     * @return True if the event was handled by the view, false otherwise.
     */
    public boolean dispatchTouchEvent(MotionEvent event) {
        //省略几行代码
        if (onFilterTouchEventForSecurity(event)) {
            //noinspection SimplifiableIfStatement
            ListenerInfo li = mListenerInfo;
            if (li != null && li.mOnTouchListener != null && (mViewFlags & ENABLED_MASK) == ENABLED
                    && li.mOnTouchListener.onTouch(this, event)) {
                return true;
            }

            if (onTouchEvent(event)) {
                return true;
            }
        }
        //省略几行代码
        return false;
    }

```
脑图分析如下

![这里写图片描述](http://img.blog.csdn.net/20160130195527465)
```
    /**
     * Implement this method to handle touch screen motion events.
     * <p>
     * If this method is used to detect click actions, it is recommended that
     * the actions be performed by implementing and calling
     * {@link #performClick()}. This will ensure consistent system behavior,
     * including:
     * <ul>
     * <li>obeying click sound preferences
     * <li>dispatching OnClickListener calls
     * <li>handling {@link AccessibilityNodeInfo#ACTION_CLICK ACTION_CLICK} when
     * accessibility features are enabled
     * </ul>
     *
     * @param event The motion event.
     * @return True if the event was handled, false otherwise.
     */
    public boolean onTouchEvent(MotionEvent event) {
        final int viewFlags = mViewFlags;

        if ((viewFlags & ENABLED_MASK) == DISABLED) {
            if (event.getAction() == MotionEvent.ACTION_UP && (mPrivateFlags & PFLAG_PRESSED) != 0) {
                setPressed(false);
            }
            // A disabled view that is clickable still consumes the touch
            // events, it just doesn't respond to them.
            return (((viewFlags & CLICKABLE) == CLICKABLE ||
                    (viewFlags & LONG_CLICKABLE) == LONG_CLICKABLE));
        }

        if (mTouchDelegate != null) {
            if (mTouchDelegate.onTouchEvent(event)) {
                return true;
            }
        }

        if (((viewFlags & CLICKABLE) == CLICKABLE ||
                (viewFlags & LONG_CLICKABLE) == LONG_CLICKABLE)) {
        	//省略一段switch语句
            return true;
        }
        return false;
    }
```

脑图分析如下：

![这里写图片描述](http://img.blog.csdn.net/20160130195650170)

##七、ViewGroup中对事件的处理

ViewGroup是View的子类，所以自然继承了View的上述两个方法。ViewGroup还重写了`dispatchTouchEvent`方法，为什么？因为啊，ViewGroup包含了多个View，事件分发时总要先判断事件落在哪个View中吧，不像非ViewGroup那样只要判断：有么有没有注册监听器啊？注册的是不是触摸监听器啊？触摸监听的回调返回值是true吗？

鉴于源码太长，个人认知有限，先理解一部分，其他的待日后研究。其中的部分代码展示了ViewGroup对事件是否要拦截的判断，**当intercepted为true时表示要拦截，dispatchTouchEvent会返回false，表示不向下分发了。**

```
// Check for interception.
final boolean intercepted;
if (actionMasked == MotionEvent.ACTION_DOWN
		|| mFirstTouchTarget != null) {
	final boolean disallowIntercept = (mGroupFlags & FLAG_DISALLOW_INTERCEPT) != 0;
	if (!disallowIntercept) {
		intercepted = onInterceptTouchEvent(ev);
		ev.setAction(action); // restore action in case it was
								// changed
	} else {
		intercepted = false;
	}
} else {
	// There are no touch targets and this action is not an initial
	// down
	// so this view group continues to intercept touches.
	intercepted = true;
}
```


子类通常具有比父类更多的方法。ViewGroup具有对其子View的生杀大权，自然有拦截事件不向子View分发的方法，即`onInterceptTouchEvent`，源码如下，意思就是**默认不拦截**。

```
/**
 * Implement this method to intercept all touch screen motion events. This
 * allows you to watch events as they are dispatched to your children, and
 * take ownership of the current gesture at any point.
 * ......共37行注释
 **/
public boolean onInterceptTouchEvent(MotionEvent ev) {
    return false;
}
```

##八、Activity对Touch事件的处理

我们在**谁来处理Touch事件？**那里分析得知：Activity持有一个Window，而Window持有一个DecorView。而事件是至上而下分发的，所以Activity对事件拥有最高的优先处理权，它可以决定是否要将时间分发给Window。

默认情况下`dispatchTouchEvent`返回值是true，分发的；当没有任何的View来处理触摸事件时，会系统调用`onTouchEvent`方法。

重写`dispatchTouchEvent`可以让它返回false，从而截断了所有的触摸事件，此时不会调用`onTouchEvent`方法。

上述两个方法的源码如下：

```
/**
 * Called to process touch screen events.  You can override this to
 * intercept all touch screen events before they are dispatched to the
 * window.  Be sure to call this implementation for touch screen events
 * that should be handled normally.
 * 
 * @param ev The touch screen event.
 * 
 * @return boolean Return true if this event was consumed.
 */
public boolean dispatchTouchEvent(MotionEvent ev) {
    if (ev.getAction() == MotionEvent.ACTION_DOWN) {
        onUserInteraction();
    }
    if (getWindow().superDispatchTouchEvent(ev)) {
        return true;
    }
    return onTouchEvent(ev);
}
```

```
    /**
     * Called when a touch screen event was not handled by any of the views
     * under it.  This is most useful to process touch events that happen
     * outside of your window bounds, where there is no view to receive it.
     * 
     * @param event The touch screen event being processed.
     * 
     * @return Return true if you have consumed the event, false if you haven't.
     * The default implementation always returns false.
     */
    public boolean onTouchEvent(MotionEvent event) {
        if (mWindow.shouldCloseOnTouch(this, event)) {
            finish();
            return true;
        }
        
        return false;
    }
```

##九、总结

写了这么多，似乎该有点总结。

1. 事件的处理是至上而下的，从Activity到ViewGroup再到非ViewGroup。
2. 理清事件处理关键在`onDispatchTouchEvent`方法。在这个方法中会做一个抉择：是要直接分发给子View处理，或是先交给自己`onTouchEvent`方法处理后再抉择。
3. ViewGroup作为容器类View，对事件的处理多了`onInterceptTouchEvent`这个阻断方法，其实我们只要看`onDispatchTouchEvent`就行了，因为它会在这个方法中调用`onInterceptTouchEvent`做是否阻断的判定。
4. 返回true，通常表示处理或消费了事件，不再传递。

----------------

><span style="font-size:12px">本文标题: <a href="{{ permalink }}">{{ title }}</a>
文章作者: <a href="http://linlshare.github.io/">林炜富</a>  
许可协议: <img alt="知识共享许可协议" style="border-width:0" src="https://i.creativecommons.org/l/by-nc-sa/4.0/80x15.png" /><a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/">©署名-非商用-相同方式共享 4.0</a></span>