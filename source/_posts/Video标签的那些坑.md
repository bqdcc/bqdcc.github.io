---
title: Video标签的那些坑
date: 2018-07-16 14:52:46
tags: [浏览器,chrome]
---

## mp4 视频 无法在 chrome 中播放

chrome 支持 H.264 格式的视频

![图1](./H.264.jpg)

为了推广H.264，MPEG-LA规定，只要你的视频用于互联网上的免费播放，就可以无偿获得使用许可证。所以YouTube可以免费使用MPEG-LA许可证。

## chrome video 无法自动播放

添加 `muted` 属性可解决

```html
    <video muted autoplay src=""></video>
```

<!--more-->

通过 页面加载完成后 play 方法 也无法实现视频自动播放 
和 视频 大小无关 仅 1m

![图2](./muted.png)

来自浏览器的限制——有声视频自动播放是不被允许的
muted autoplay is always allowed
高版本浏览器，对视频静音后，可以保证视频自动播放。

