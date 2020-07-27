---
layout: post
title: "Springboot 引入本地jar包"
date: 2020-03-18 
description: "Springboot 引入本地jar包"
tag: Springboot
---

# 配置Maven 环境变量
1. 下载Maven包（http://maven.apache.org/download.cgi）
2.  解压后添加环境变量：
  变量名：MAVEN_HOME
  变量值：D:\maven\apache-maven-3.6.3
3. Path中配置引导路径：%MAVEN_HOME%\bin  
4. 验证Maven环境是否配置成功：mvn -v （mvn -version）  

# Springboot 引入 
1.首先在根目录添加lib包，把jar放这里，右键jar包Add as library
2.执行以下命令, 以ttjdbc11.jar为例子
```
mvn install:install-file -Dfile=ttjdbc11.jar  -DgroupId=cn.wzhospital.timesten -DartifactId=timesten -Dversion=11.0.0 -Dpackaging=jar
```
注意：要在lib文件目录下操作 不然会找不到这个jar包
3.pom.xml引入依赖
```
<dependency>
    <groupId>cn.wzhospital.timesten</groupId>
    <artifactId>timesten</artifactId>
    <version>11.0.0</version>
</dependency>
```
