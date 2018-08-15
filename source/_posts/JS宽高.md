---
title: JS宽高
date: 2018-08-15 09:21:43
tags: JavaScript
---

## 你可明白

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
内部宽高和外部宽高
`window.innerWidth; window.innerHeight; window.outerWidth; window.outerHeight`
![innerWidth&outerWidth](./innerWidth&outerWidth.png)
![innerHeight&outerHeight](./innerHeight&outerHeight.png)

## 参考
[JS/jQuery宽高的理解和应用](https://www.imooc.com/learn/608)
[JavaScript中的各种宽高以及位置总结](https://segmentfault.com/a/1190000002545307)