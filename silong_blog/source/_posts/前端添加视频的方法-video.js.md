---
title: 前端添加视频的方法
abbrlink: 'videoFronted'
categories: 前端
tags: [前端, video.js]
---
# 前端添加视频的方法-video.js

## 纯video.js

### 下载与引用

首先下载并引用video.js： CDN方法 略 （引用的时候js和css文件都要引用）

npm方法：  npm install video.js

### 简单的使用

引用之后直接使用标签 `video`  ，然后进行标签初始化：

1.第一种初始化方法：标签内加上 class="video-js" 和 data-setup='{}'

2.JS初始化

```
var player = videojs('example_video')
//player是变量名，videojs是前面已经引用过的，example_video是标签video的id名字
```

然后再video标签内添加source地址 进行视频的路径导向。

参考文章：[戳这里](https://blog.csdn.net/little__SuperMan/article/details/89203270)

### 样式改变

比如播放按钮居中，播放按钮变成圆形之类的样式要求。

参考文章：[戳这里](https://www.awaimai.com/2053.html)

## vue中使用video.js --> vue-video-js

vue其实有基于video.js封装的 vue-video-player组件，直接安装就可以了

进行安装：

```
npm install vue-video-player --saver
```

然后在main.js中引入：

```javascript
import Vue from 'vue'
import VueVideoPlayer from 'vue-video-player'

// require videojs style
import 'video.js/dist/video-js.css'
// import 'vue-video-player/src/custom-theme.css'
// import 'vue-video-player/node_modules/video.js/dist/video-js.css'

Vue.use(VueVideoPlayer, /* {
  options: global default options,
  events: global videojs events
} */)
//Vue.prototype.$vueVideoPlayer = vueVideoPlayer;
```

**在页面中引用：**

```
<video-player  class="video-player vjs-custom-skin"
  ref="videoPlayer" 
  :playsinline="true" 
  :options="playerOptions"
></video-player>
//ref很重要，操作video-player的时候都是通过refs
//playerOptions 里面的参数就是video-player的参数配置,这里面的options就是videojs的options配置
```

options配置参考：[戳这里](https://docs.videojs.com/tutorial-options.html)

**配置数据（playerOptions）**

```
//常见配置
playerOptions : {
    playbackRates: [0.7, 1.0, 1.5, 2.0], //播放速度
    autoplay: false, //如果true,浏览器准备好时开始播放。
    muted: false, // 默认情况下将会消除任何音频,也就是静音播放。
    loop: false, // 视频播放结束时候循环播放。
    preload: 'auto', // 建议浏览器在<video>加载元素后是否应该开始下载视频数据。auto浏览器选择最佳行为,立即开始加载视频（如果浏览器支持）
    language: 'zh-CN',
    aspectRatio: '16:9', // 将播放器置于流畅模式，并在计算播放器的动态大小时使用该值。值应该代表一个比例 - 用冒号分隔的两个数字（例如"16:9"或"4:3"）
    fluid: true, // 当true时，Video.js player将拥有流体大小。换句话说，它将按比例缩放以适应其容器。
    sources: [{
      type: "",//这里的种类支持很多种：基本视频格式、直播、流媒体等，具体可以参看git网址项目
      src: "" //url地址。视频地址，可以是录制好的视频，也可以是直播，这由type进行控制
    }],
    poster: "../../static/images/test.jpg", //你的封面地址
    // width: document.documentElement.clientWidth, //播放器宽度
    notSupportedMessage: '此视频暂无法播放，请稍后再试', //允许覆盖Video.js无法播放媒体源时显示的默认信息。
    controlBar: {//视频的控制栏
      timeDivider: true,
      durationDisplay: true,
      remainingTimeDisplay: false,
      fullscreenToggle: true  //全屏按钮
    }
}
```



有video 也就有audio 感兴趣的话可以去搜搜