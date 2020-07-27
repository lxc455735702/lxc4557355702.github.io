---
layout: post
title: "cube-ui 自定义dialog一闪而过的问题"
date: 2019-11-07 
description: "cube-ui 自定义dialog一闪而过的问题"
tag: vue
---


# 问题描述  
之前这个 dialog 在测试页的时候是能正常弹出并且显示的。  
 某个页面点击某个div时弹出自定义的dialog，发现这个dialog 并没  有显示，有点奇怪。
 
![](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy80ODMwNjY0LTE3Mjk5YWU0MzkwYjM5NjAuanBn?x-oss-process=image/format,png)
---
# 问题排查
  一开始觉得有可能是dialog显示了被遮挡住了，所以看起来没有显示。然后修改z-index属性，发现还是一样的效果。
看来问题没有这么简单，还是仔细的分析一下...
最外层是个cube-slide,cube-slide-item 里面的div 触发点击弹出 自定义的dialog。dialog本身显示是没有问题的，可能是受到了其它组件的影响。然后我增加了一些打印，dialog的显示和隐藏的函数进行打印。
![](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy80ODMwNjY0LWM5MWE1NjhjM2ZhMGY3NWMucG5n?x-oss-process=image/format,png)
打印看到的时候，才发现原来是dialog被隐藏了..
是不是代码哪里调用了hidedialog函数，发现并没有地方主动调用。

仔细想想感觉应该是导致了点击事件触发了两次。也只有可能是cube-slide会影响。带着问题继续去探索，cube-slide 是一个基于 better-scroll 进行封装的组件。
options 是 better-scroll 配置项，项目里的代码配置如下：
```
slideOptions: {
        listenScroll: true,
        probeType: 3,
        directionLockThreshold: 0,
        eventPassthrough: 'vertical'
      }
```
![](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy80ODMwNjY0LWQ3NThiMGIwOThmYjA4MzkucG5n?x-oss-process=image/format,png)

click默认是没有影响的。

![](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy80ODMwNjY0LTQ4NTBkYWNhNjEzMTkyNWQucG5n?x-oss-process=image/format,png)

这个属性我注释掉了，发现 dialog  终于显示出来了。。
但是这个属性，我觉得也是有必要加的然后就增加了click:false
dialog也是可以正常显示的。

# 总结 
这个问题是同事让我帮忙看的，排查问题还是要一点点去慢慢确认问题。这种看似很奇怪的问题，更需要耐心。







