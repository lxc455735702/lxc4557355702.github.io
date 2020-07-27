---
layout: post
title: "nuxt+cube-ui主题色修改"
date: 2019-07-11 
description: "nuxt+cube-ui主题色修改"
tag: vue
---

# 前言
  因为刚接触nuxt, 对于nuxt.config.js 里面的配置不是很清楚该怎么弄，而且cube-ui的主题色需要后编译去解决。所以还是提了[Issues](https://github.com/didi/cube-ui/issues/520)。

# 官方示例
[点击查看](https://github.com/cube-ui/cube-nuxt-demo)

# 问题
1.克隆项目本地后报错 上面的issues也有体现。

解决方案：用 NUXT 脚手架初始化的的项目，nuxt 被放在了 dependencies 下，需要手动移动到 devDependencies 下，否则 windows 下有坑。 

2.创建nuxt项目，在选择 custom server framework 的时候 选择了 Koa 。然后再按照官方示例的配置去配置还是遇到了报错。

解决方案： 不要配置 server framework 直接选择none的情况是好的。当然我也提了[示例仓库的Issues](https://github.com/cube-ui/cube-nuxt-demo/issues/1)，我也搜了相关报错的博客，说是因为语法的问题。为了推进自己的项目还是选择重新不配置koa，移动了代码。

# 总结
 当时第二个问题挺纠结的，因为我看官方的示例是好的。自己的项目也是一样的配置，但是一直有问题。一直没解决。对比出来发现多了Koa的配置。因为当初也是想选择koa留着之后用。没想到...

不过最终还是在nuxt 可以修改cube-ui的主题色，也是蛮ok的。