---
layout: post
title: "Android Webview File同源策略绕过漏洞"
date: 2020-05-12
description: "Android Webview File同源策略绕过漏洞"
tag: 移动安全
---


| 测评目的 | 检测Apk中WebView的file域协议是否存在同源策略绕过的漏洞 |
|--|--|
| 危险等级 | 高 |
| 危害 | JavaScript的延时执行能够绕过file协议的同源检查，并能够访问受害应用的所有私有文件，即通过WebView对Javascript的延时执行和将当前Html文件删除掉并软连接指向其他文件就可以读取到被符号链接所指的文件，然后通过JavaScript再次读取HTML文件，即可获取到被符号链接所指的文件。大多数使用WebView的应用都会受到该漏洞的影响，恶意应用通过该漏洞，可在无特殊权限下盗取应用的任意私有文件，尤其是浏览器，可通过利用该漏洞，获取到浏览器所保存的密码、Cookie、收藏夹以及历史记录等敏感信息，从而造成敏感信息泄露。 |
| 测评结果 | 存在风险(发现4处) |
| 测评结果描述 | 该Apk程序存在webview File同源策略绕过的漏洞 |
| 测评详细信息 | ... （省略具体位置） |

# 解决方案
1.对于不需要使用 file 协议的应用，禁用 file 协议
```
setAllowFileAccess(false);
```
2.对于需要使用 file 协议的应用，禁止 file 协议加载 JavaScript 
```
setAllowFileAccess(true); 

// 禁止 file 协议加载 JavaScript
if (url.startsWith("file://") {
    setJavaScriptEnabled(false);
} else {
    setJavaScriptEnabled(true);
}
```
