---
layout: post
title: "ScrollView嵌套RecyclerView导致onBindViewHolder不断调用"
date: 2019-05-30 
description: "ScrollView嵌套RecyclerView导致onBindViewHolder不断调用"
tag: Android
---

## 描述：
当Recyclerview 外部嵌套了一层可滑动布局时，RecyclerView 的回收复用机制将失效。
在数据量小的时候不明显；等数据量达到一定程度的时候就会导致创建的View过多，产生大量的数据，导致进程不断的发生GC影响UI主线程,会造成无响应，卡顿的问题。

## 原因：
RecyclerView默认是支持嵌套滚动的,也就是说当它嵌套在ScrollView中时,默认会随着ScrollView滚动而滚动，RecyclerView滚动无效。这就导致RecyclerView绘制的view不能被回收。

## 解决:
去掉外层的ScrollView,将滑动的布局添加到RecyclerView中。