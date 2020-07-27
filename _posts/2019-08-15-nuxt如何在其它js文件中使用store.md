---
layout: post
title: "nuxt如何在其它js文件中使用store"
date: 2019-08-15 
description: "nuxt如何在其它js文件中使用store"
tag: vue
---

# 前言
  在新建的js文件中想用store里面的数据，比如token想在封装的axios里面，请求头里面去使用，亦或者通过app的JS接口获取token并存储在store里面。我们都知道如何在vue中如何使用。

# 代码
```
/*
 * @Description: 
 * @Author: lxc
 * @Date: 2019-07-02 16:14:07
 * @LastEditTime: 2019-08-14 16:08:19
 * @LastEditors: lxc
 */
// 导出 store 的地方

import Vue from 'vue'
import Vuex from 'vuex'
import state from './state'
import actions from './actions'
import mutations from './mutations'
import getters from './getters'
import canteen from './modules/canteen'
import contact from './modules/contact'
import health from './modules/health'
import scan from './modules/scan'

Vue.use(Vuex)

let store

const initStore = () => {
  return store || (store = new Vuex.Store({
    // 存放公用数据
    state,
    // 异步操作要通过actions，否则通过cimmit直接操作mutations
    actions,
    // 同步放数据
    mutations,
    getters,
    modules: {
      // store 模块....
    }
  }))
}

export default initStore

```
其它js文件中如何调用:
```
import store from '@/store'

const TOKEN = 'testToken'

//  这里只是举个例子
function getToken() {
  return isNotEmpty(store().state.token) ? store().state.token : TOKEN
}

```
我自己使用是可以的。希望对大家有帮助。