---
title: VUEX使用方法
abbrlink: 'vuex1.0'
categories: vue
tags: [vue, vuex]
---
# VUEX的使用

[官网文档](https://vuex.vuejs.org/zh/guide/testing.html)

[看这里](https://www.jianshu.com/p/2e5973fe1223)

只能通过在mutation，getter，action中写逻辑来改变state中的数据。并且不能直接通过 = 来实现，需要使用 vue 中的 set delete方法

mutation只能写同步方法，异步都写在action中

调用mutation的时候 需要用 `store.commit('mutation')`方法

调用action的时候，需要用`store.dispatch('action')`方法