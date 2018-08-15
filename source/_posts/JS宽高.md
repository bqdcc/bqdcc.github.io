---
title: JS宽高
date: 2018-08-15 09:21:43
tags: JavaScript
---

## 两个问题

1.`window` 和 `document` 的区别?
[window](https://developer.mozilla.org/zh-CN/docs/Web/API/Window) 表示浏览器中打开(包含DOM文档)的窗口
window 是可以省略的,如 `alert();window.alert()`

[document](https://developer.mozilla.org/zh-CN/docs/Web/API/Document) 表示任何在浏览器中已经加载好的网页，并作为一个入口去操作网页内容（也就是DOM tree）
document 对象是 window 对象的一部分,如 `document.body;window.document.body`

2.`window.location` 和 `document.location` 一样吗?
[window.location](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/location) 只读属性，返回一个 Location 对象,表示该窗口当前显示文档的URL相关信息

[document.location](https://developer.mozilla.org/zh-CN/docs/Web/API/Document/location) 只读属性，返回一个 Location 对象,表示该文档的URL相关信息
`window.location === document.location;//true` 即使在 iframe 中也是一样的

    尽管 window.location 是一个只读 Location 对象，你仍然可以赋给它一个 DOMString。
    这意味着您可以在大多数情况下处理 location，就像它是一个字符串一样：window.location = 'http://www.example.com'，
    是 window.location.href = 'http://www.example.com'的同义词 

<!--more-->

## window 相关宽高
单位:像素
### 内部和外部 宽高
[window.innerWidth](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/innerWidth) 
[window.innerHeight](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/innerHeight) 
[window.outerWidth](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/outerWidth) 
[window.outerHeight](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/outerHeight)

![innerWidth&outerWidth](./innerWidth&outerWidth.png)
![innerHeight&outerHeight](./innerHeight&outerHeight.gif)

    window.innerWidth:浏览器视口（viewport）宽度（单位：像素），如果存在垂直滚动条则包括它。

大部分情况下 `innerWidth&outerWidth`是相等的
`innerWidth&innerHeight` 表示的是浏览器视口(viewport)宽高, 故受缩放影响(浏览器缩放,windowsDPI设置)
![缩放innerWidth](./缩放innerWidth.png)

### [Screen](https://developer.mozilla.org/zh-CN/docs/Web/API/Screen) 相关信息
[window.screen.availHeight](https://developer.mozilla.org/zh-CN/docs/Web/API/Screen/availHeight): 浏览器窗口在屏幕上可占用的垂直空间
[window.screen.availWidth](https://developer.mozilla.org/zh-CN/docs/Web/API/Screen/availWidth): 浏览器窗口在屏幕上可占用的水平空间
[window.screen.width](https://developer.mozilla.org/zh-CN/docs/Web/API/Screen/width): 屏幕的宽度
[window.screen.height](https://developer.mozilla.org/zh-CN/docs/Web/API/Screen/height): 屏幕的高度
[window.screenTop](https://www.w3cschool.cn/jsref/prop-win-screenleft.html): 浏览器窗口相对于屏幕顶部的距离
[window.screenLeft](https://www.w3cschool.cn/jsref/prop-win-screenleft.html): 浏览器窗口相对于屏幕左边的距离
[window.screen.availTop](https://developer.mozilla.org/zh-CN/docs/Web/API/Screen/availTop): 浏览器窗口在屏幕上的可占用空间上边距离屏幕上边界的像素值
[window.screen.availLeft](https://developer.mozilla.org/zh-CN/docs/Web/API/Screen/availLeft): 浏览器可用空间左边距离屏幕（系统桌面）左边界的距离

![screen.height&screen.availHeight](./screen.height&screen.availHeight.gif)
![screen](./screen.jpg)


## 参考
[JS/jQuery宽高的理解和应用](https://www.imooc.com/learn/608)
[JavaScript中的各种宽高以及位置总结](https://segmentfault.com/a/1190000002545307)

