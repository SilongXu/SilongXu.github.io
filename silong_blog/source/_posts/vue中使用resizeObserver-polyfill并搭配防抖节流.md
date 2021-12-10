---
title: resiezeObserver-lodash搭配
abbrlink: 'resizeLodash'
categories: 前端
tags: [vue, 前端, resizeObserver, lodash-es]
---
# vue中使用resizeObserver-polyfill并搭配防抖节流

## resizeObserver-polyfill

### resizeObserver-polyfill的作用

resizeObserver-polyfill 的github地址：[resizeObserver-polyfill](https://github.com/que-etc/resize-observer-polyfill)

npm安装：当我们需要知道一个元素的大小变化或者屏幕横竖屏时，我们需要监听window.resize事件或者window.orientationchange方法。由于reize事件会在一秒内触发将近60次，所以很容易在改变窗口大小时导致性能问题。换句话说，window.resize事件通常是浪费的，因为它会监听每个元素的大小变化（只有window对象才有resize事件），而不是具体到某个元素的变化。如果我们只想监听某个元素的变化的话，这种操作就很浪费性能了。

而ResizeObserver API就可以帮助我们：监听一个DOM节点的变化，这种变化包括但不仅限于：

1. 某个节点的出现和隐藏
2. 某个节点的大小变化

ResizeObserver API是一个新的JavaScript API，与IntersectionObserver API非常相似，它们都允许我们去监听某个元素的变化。

### resizeObserver-polyfill的使用

#### 安装

```
npm install resize-observer-polyfill --save-dev
```

#### 使用

基本使用方法：

```javascript
import ResizeObserver from 'resize-observer-polyfill';

const ro = new ResizeObserver((entries, observer) => {
    for (const entry of entries) {
        const {left, top, width, height} = entry.contentRect;

        console.log('Element:', entry.target);
        console.log(`Element's size: ${ width }px x ${ height }px`);
        console.log(`Element's paddings: ${ top }px ; ${ left }px`);
        //在此处做你要做的事情，例如我得到了header的自身的高度，然后设置Drawer的margin-top为这个高度，那么，无论header如果变化，我的drawer都会紧紧吸附于header了

    }
});

ro.observe(document.body); //这里是你观察的对象，官方例子观察body，我观察的是header的容器 
```

## lodash-es的函数节流和防抖

放上lodash的官网地址：[lodash-es](https://www.lodashjs.com/docs/lodash.debounce)

里面不仅仅又节流和防抖，还有很多别的JS使用组件工具

### 函数节流

#### 什么是函数节流

节流就是指限定一个函数的调用频率限制在一个阈值内，例如1s内某个函数不能被调用两次

#### 函数节流的使用方法

lodash提供了这个方法

```javascript
_.throttle(func, [wait=0], [options={}])
```

函数节流的具体使用例子：

```javascript
// 避免在滚动时过分的更新定位
jQuery(window).on('scroll', _.throttle(updatePosition, 100));

// 点击后就调用 `renewToken`，但5分钟内超过1次。
var throttled = _.throttle(renewToken, 300000, { 'trailing': false });
jQuery(element).on('click', throttled);

// 取消一个 trailing 的节流调用。
jQuery(window).on('popstate', throttled.cancel);
```

### 函数的防抖

#### 什么是函数的去抖

去抖指的是当调用函数n秒后，才会执行该动作，若在这n秒内又调用该函数则将取消前一次并重新计算执行时间，举个简单的例子，我们要根据用户输入做suggest，每当用户按下键盘的时候都可以取消前一次，并且只关心最后一次输入的时间就行了。

lodash也提供了这个方法

```javascript
_.debounce(func, [wait=0], [options={}])
```

防抖的简单用例：

```javascript
// 避免窗口在变动时出现昂贵的计算开销。
jQuery(window).on('resize', _.debounce(calculateLayout, 150));

// 当点击时 `sendMail` 随后就被调用。
jQuery(element).on('click', _.debounce(sendMail, 300, {
  'leading': true,
  'trailing': false
}));

// 确保 `batchLog` 调用1次之后，1秒内会被触发。
var debounced = _.debounce(batchLog, 250, { 'maxWait': 1000 });
var source = new EventSource('/stream');
jQuery(source).on('message', debounced);

// 取消一个 trailing 的防抖动调用
jQuery(window).on('popstate', debounced.cancel);
```

## resizeObserver-polyfill和lodash的搭配

简单的举例即可：

```javascript
// throttle需要自行引入哈
const myObserver = new ResizeObserver(throttle(entries => {
  entries.forEach(entry => {
    console.log('大小位置 contentRect', entry.contentRect)
    console.log('监听的DOM target', entry.target)
  })
}), 500)
```

