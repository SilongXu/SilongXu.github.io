---
title: VUE实现元素高度根据屏幕高度动态改变/overflow踩坑
abbrlink: 'autoHeightForElement'
categories: vue
tags: [前端, vue]
---
# VUE实现元素高度根据屏幕高度动态改变/overflow踩坑

## overflow踩坑

通常我们会在需要实现滚动条的时候添加overflow这个属性。必须要给赋予overflow的元素一个固定的height或者width才能实现对应方向上的滚动条。

## vue实现元素高度根据屏幕高度动态改变

先感谢这篇博客：[https://www.cnblogs.com/wuqilang/p/13434823.html](https://www.cnblogs.com/wuqilang/p/13434823.html) 

首先在data中给定一个body的高度：

```
  data() {
    return {
      clientHeight: document.body.clientHeight
    }
  }
```

然后在mounted中在每次载入页面的时候都先给定一个初始值：

```javascript
  mounted() {
    const that = this
    window.onresize = () => {
      return (() => {
        window.screenHeight = document.body.clientHeight
        that.clientHeight = window.screenHeight
      })()
    }
  },
```

接着使用watch函数监听clientHeight的变化：

```javascript
  watch: {
    clientHeight(val) {
      // 为了避免频繁触发resize函数导致页面卡顿，使用定时器
      if (!this.timer) {
        // 一旦监听到的screenWidth值改变，就将其重新赋给data里的screenWidth
        this.clientHeight = val
        this.timer = true
        let that = this
        setTimeout(function() {
          // 打印screenWidth变化的值
          console.log(that.clientHeight)
          that.timer = false
        }, 400)
      }
    }
  }
```

最后在需要动态获取高度（宽度）的元素上动态获取屏幕的可视高度：

```javascript
  :style="{ height: clientHeight-194 + 'px' }"
```

总结就是使用 :style 动态改变元素的样式  然后使用watch函数监听元素样式的值的变化