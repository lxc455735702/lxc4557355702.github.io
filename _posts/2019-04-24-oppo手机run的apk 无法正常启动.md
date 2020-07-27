---
layout: post
title: "oppo手机run的apk 无法正常启动 Caused by: java.lang.ClassNotFoundException DexPathList[ */base.apk]"
date: 2019-04-24 
description: "oppo手机run的apk 无法正常启动 Caused by: java.lang.ClassNotFoundException DexPathList[ */base.apk]"
tag: Android开发常见错误
---

## 前言
最近新到了几部测试机,迫不及待的运行项目。
之后就发现oppo 和  vivo 的手机上运行的apk 并不能正常启动。一脸懵逼，之前都是好好的，怎么会突然出现问题呢。

## 错误日志
```
Caused by: java.lang.ClassNotFoundException:
Didn't find class "项目包名" on path: 
DexPathList[[zip file "/data/app//项目包名-F67befhdjRrY-P-bPPOC-w==/base.apk"],
nativeLibraryDirectories=[/data/app/项目包名-F67befhdjRrY-P-bPPOC-w==/lib/arm, 
/data/app/项目包名-F67befhdjRrY-P-bPPOC-w==/base.apk!/lib/armeabi-v7a, /system/lib, /vendor/lib]]
```
怎么会没有base.apk...？？

## 解决方案
查阅了资料，需要关闭Instant run 
如下图，把红色框内的勾去掉就好。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190424134758948.PNG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2dpdGh1Yl8zODc4NzYxNQ==,size_16,color_FFFFFF,t_70)
也许还是因为在这一类的手机上，可能安装的包并不完整导致的。
还是需要看一下 [instant run 工作原理](https://www.jianshu.com/p/2e23ba9ff14b)。