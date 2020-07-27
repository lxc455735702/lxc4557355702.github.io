---
layout: post
title: "Invalid escape sequence at line 1 column 29 path $[0].name"
date: 2019-04-24 
description: "Invalid escape sequence at line 1 column 29 path $[0].name"
tag: Android开发常见错误
---

```
Invalid escape sequence at line 1 column 29 path $[0].name
```
每次运行的时候老是会出现这个错误。

## 解决方案
项目下 gradle.properties 文件中 添加

```
 org.gradle.jvmargs = -Dfile.encoding=UTF-8
```
如下图：
![](https://img-blog.csdnimg.cn/20190424142145237.PNG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2dpdGh1Yl8zODc4NzYxNQ==,size_16,color_FFFFFF,t_70)