---
layout: post
title: "cube-ui form控件input增加金钱规则"
date: 2019-07-16 
description: "cube-ui form控件input增加金钱规则"
tag: vue
---


# 前言
  因为rules类型只有 'string', 'number', 'array', 'date', 'email', 'tel', 'url',没有支持金钱类型。用number 的话没有精度，所以一般不适用，那只能看官方文档来自己添加一种规则 [官方文档](https://didi.github.io/cube-ui/#/zh-CN/docs/validator)

# 添加规则
```
import Vue from 'vue'
// 当然这里还有一些其它组件的引入我只是举个例子
import { Validator } from 'cube-ui'
Vue.use(Validator)
Validator.addRule('money', (val, config, type) => typeof !config && /(^[1-9]([0-9]+)?(\.[0-9]{1,2})?$)|(^(0){1}$)|(^[0-9]\.[0-9]([0-9])?$)/i.test(val))
Validator.addMessage('money', '请输入正确的金钱格式，精确到分')
```
# 金钱匹配正则
 正则我是网上看博客随便找的，只是为了测试一下。
```
/(^[1-9]([0-9]+)?(\.[0-9]{1,2})?$)|(^(0){1}$)|(^[0-9]\.[0-9]([0-9])?$)/
```
ps : 推荐一个比较好用的网站，来检验[正则显示](https://regexper.com/#%2Fhttps%3F%3A%5C%2F%5C%2F%2Fig)
# 使用

```
 fields: [
        {
          type: 'input',
          modelKey: 'money',
          label: '充值金额',
          props: {
            placeholder: '请输入充值金额'
          },
          rules: {
            required: true,
            type: 'number',
            min: 0,
            money: true
          }
        }
      ]
```
# 效果
![效果图](https://img-blog.csdnimg.cn/20190716104803147.gif)

# 总结 
 因为正则没有匹配 - 符号，所以我先把min的规则放在money之前，不然会先提示money的提示语，只是一个简单的例子。有不足的地方欢迎指正~谢谢。

 