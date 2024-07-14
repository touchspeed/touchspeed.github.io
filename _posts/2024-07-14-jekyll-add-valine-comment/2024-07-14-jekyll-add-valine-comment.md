---
layout: post
title: 为 Jekyll 添加 Valine 评论系统
date: 2024-07-14 13:20
category: Other
author: Meve
tags: [Tips]
summary: 
---

为博客添加评论系统 - valine

## 准备

首先按照[valine官方说明](https://valine.js.org/quickstart.html)注册LeanCloud帐号获取AppID和AppKey

![alt text](https://raw.githubusercontent.com/touchspeed/touchspeed.github.io/main/_posts/2024-07-14-jekyll-add-valine-comment/1.png)

## 开始

我的博客页面样式文件是_layouts/post.html文件，评论样式文件是_include\valine.html文件

先在_include\文件夹创建valine.html
![alt text](https://raw.githubusercontent.com/touchspeed/touchspeed.github.io/main/_posts/2024-07-14-jekyll-add-valine-comment/image.png)

然后在_config.yml添加以下变量内容

![alt text](https://raw.githubusercontent.com/touchspeed/touchspeed.github.io/main/_posts/2024-07-14-jekyll-add-valine-comment/image-1.png)

然后在需要开启评论的页面样式文件中合适位置添加调用valine评论系统，例如_layouts/post.html

![alt text](https://raw.githubusercontent.com/touchspeed/touchspeed.github.io/main/_posts/2024-07-14-jekyll-add-valine-comment/2.png)

## 结束