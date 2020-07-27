---
layout: post
title: "Android Webview明文存储密码风险"
date: 2020-05-12
description: "Android Webview明文存储密码风险"
tag: 移动安全
---


| 测评目的 | 检测App应用的Webview组件中是否使用明文保存用户名及密码。 |
|--|--|
| 危险等级 | 高 |
| 危害 | Android的Webview组件中默认打开了提示用户是否保存密码的功能，如果用户选择保存，用户名和密码将被明文存储到该应用目录databases/webview.db中。而本地明文存储的用户名和密码，不仅会被该应用随意浏览，其他恶意程序也可能通过提权或者root的方式访问该应用的webview数据库，从而窃取用户登录过的用户名信息以及密码。 |
| 测评结果 | 存在风险(发现4处) |
| 测评结果描述 | 该App应用的Webview组件中未设置关闭自动保存密码功能，用户名和密码可能被明文存储。 |
| 测评详细信息 | ... （省略具体位置） |
| 解决方案 | 用到webview的时候 webView.getSettings().setSavePassword(false)即可|

