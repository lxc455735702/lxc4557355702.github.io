---
layout: post
title: "Android与js交互"
date: 2019-05-15 
description: "Android与js交互"
tag: Android
---


话不多说，直接上代码。
这里只是介绍一种简单的方式去实现与js交互。

# 客户端
```
webView.addJavascriptInterface(new JsInterface(),"jsBridge");
```
```
public class JsInterface {

    @JavascriptInterface
    public String getUserToken(){
        return UserData.getBaseUser().getToken();
    }
}
```

# vue 
```
methods: {
    getToken() {
      var token = jsBridge.getUserToken();
      console.log("getUserToken token = " + token);
      if (token) {
        this.data = token;
        localStorage.setItem("token", token);
      } else {
        console.log("not save token");
      }
    },
}
```
# 测试效果
![测试效果](https://img-blog.csdnimg.cn/20190515092720536.gif)
