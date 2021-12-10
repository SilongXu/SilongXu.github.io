---
title: 大文件上传方案
abbrlink: 'vueSimpleUploader'
categories: 前端
tags: [vue, 前端, 上传]
---
# vue-simple-uploader实现大文件上传

## 关于vue-simple-uploader

`vue-simple-uploader`是基于 `simple-uploader.js` 封装的vue上传插件。它的优点包括且不限于以下几种：

- 支持文件、多文件、文件夹上传；支持拖拽文件、文件夹上传
- 可暂停、继续上传
- 错误处理
- 支持“秒传”，通过文件判断服务端是否已存在从而实现“秒传”
- 分块上传
- 支持进度、预估剩余时间、出错自动重试、重传等操作

需要熟悉simple-uploader.js 以及 vue-simple-uploader文档：

[vue-simple-uploader文档](https://github.com/simple-uploader/vue-uploader/blob/master/README_zh-CN.md)

[simple-uploader.js文档](https://github.com/simple-uploader/Uploader/blob/develop/README_zh-CN.md)

## 安装+配置vue-simple-uploader

npm安装命令：

**npm install vue-simple-uploader --save**

然后在main.js中引用：

**import uploader from 'vue-simple-uploader'**

**Vue.use(uploader)**

安装md5工具：

**npm install spark-md5 --save**

引用md5工具：

**import SparkMD5 from "spark-md5"**

## 使用vue-simple-uploader

需要的封装：

封装一个globaluploader的vue组件

在app.vue中配置使用globaluploader

配置Bus(全局上传插件)

配置上传类型(accept_config)

参考 [封装globaluploader](https://github.com/shady-xia/Blog/tree/master/vue-simple-uploader)

(需要配合后端的接口 得到上传的url以及必要的切片配置)