---
layout: post
title: "cube-ui dialog嵌套slide遇到的问题"
date: 2019-09-06
description: "cube-ui dialog嵌套slide遇到的问题"
tag: vue
---


# 前言
  dialog去嵌套slide的场景确实不多。不过还是尝试了一下。
# 问题
 自定义了一个 TestDialog
```
<!--
 * @Description: cube-ui 嵌套 cube-slide
 * @Author: lxc
 * @Date: 2019-07-30 08:09:22
 * @LastEditTime: 2020-07-27 16:11:00
 * @LastEditors: lxc
 -->
<template>
  <transition name="cube-dialog-fade">
    <cube-popup
      ref="myPopup"
      type="dialog"
      :mask="true"
      :center="true"
      :visible="true"
      class="popup"
      @mask-click="hideDialog"
    >
      <div class="dialog-main">
        <div>测试</div>
        <div class="test">
          <cube-slide ref="mySlide" :data="items" />
        </div>
      </div>
    </cube-popup>
  </transition>
</template>

<script>
export default {
  name: 'TestDialog',
  props: {
    items: {
      type: Array,
      default: null
    }
  },
  mounted() {
    console.log('mounted')
    /* const vm = this
    this.$nextTick(function() {
      vm.$refs.mySlide.refresh()
    }) */
  },
  methods: {
    hideDialog(e) {
      this.$refs.myPopup.hide()
    },
    // 必须写在这里
    show() {
      this.$refs.myPopup.show()
    }
  }
}
</script>

<style lang="stylus" scoped>
@import '~styles/color';

.popup {
  width: 100%;

  .dialog-main {
    width: 20rem;
    padding: 1rem;
    overflow: hidden;
    border-radius: 0.2rem;
    background-color: white;

    .test {
      width: 100%;
    }
  }
}
</style>
```
却发现并不能正常的显示slide 如：下图 
![问题.png](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy80ODMwNjY0LTRmNmJhZTBkZGM0ZTNiNGYucG5n?x-oss-process=image/format,png)

# 解决
初始化的时候的宽高slide并不知道父级元素什么时候从display none 到了可见状态 所以里边的内容不对
与官方cube-ui交流群探讨，得到了帮助，解决了该问题。
感谢 cube-苗典
```
mounted() {
    console.log('mounted')
    const vm = this
    this.$nextTick(function() {
      vm.$refs.mySlide.refresh()
    })
  },
```
刷新了slide之后显示就正常了。 在此记录一下。希望有遇到相同问题的同学得到帮助。




