---
layout: post
title: "Spring boot 新建项目 Spring Initializr Error 解决方案"
date: 2020-05-28
description: "Spring boot 新建项目 Spring Initializr Error 解决方案"
tag: Springboot
---

# 1.问题 Spring Initializr Error 
 Spring boot  新建项目的时候遇到问题Spring Initializr Error 如下图：
 ![错误信息](https://img-blog.csdnimg.cn/20200528134207294.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2dpdGh1Yl8zODc4NzYxNQ==,size_16,color_FFFFFF,t_70#pic_center)
 # 2.解决方案
 

 1. 第一步，点开file-> setting
 2. 第二步，选择 HTTP Proxy
 3. 第三步，勾选 Auto-detect proxy settings
 4. 第四步，点击Check connection，弹框中 输入 https://start.spring.io 或http://start.spring.io  
 5. 弹出 successful 即可  
 
ps : 我一开始输入网址的时候 提示连接超时，过一会它自己就好了 

![解决方案](https://img-blog.csdnimg.cn/20200528134546641.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2dpdGh1Yl8zODc4NzYxNQ==,size_16,color_FFFFFF,t_70#pic_center)
