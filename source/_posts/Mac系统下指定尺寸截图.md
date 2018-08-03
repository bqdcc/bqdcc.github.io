---
title: Mac系统下指定尺寸截图
date: 2018-05-09 14:18:40
tags: MacOs
---

遇到一个需求,需要截取网页上多张指定位置指定尺寸的图片,我想这简单,找一个可以设置截图矩形框的截图软件,自己在将矩形框挪到我想要的位置不就可以了(虽然累了点),
然后我就去找啊找啊...两个小时就过去了,居然找不到!

## screencapture
[不可不知的Mac OS X专用命令行工具(持续更新中)](https://blog.ihoey.com/posts/Mac/2017-11-21-Mac-OS-X-command-line-tool.html)
通过此篇博文了解到了 screencapture 


截取屏幕 指定位置指定尺寸图片 并保存

    screencapture -R 90,340,660,540 -P ./市禽畜粪便产气量.png

其中 -R<x,y,w,h> 捕捉矩形区域 
-P屏幕截图将在预览中打开
./市禽畜粪便产气量.png 保存的图片名称及路径 此处为当前文件夹


