---
layout: post
title: rpm update wps
date: 2024-07-11 17:50
category: Linux 
author: Meve
tags: [Linux,Tips]
summary: 
---


## 前言

今天使用rocky时，发现wps有更新，每次打开都提示，那就更新一下吧。

***BTW：wps for linux会一直保持干净免费吗...***

## 开始

去 [wps for linux官网](https://linux.wps.cn/) 下载一下rpm包，准备安装。

![alt text](https://raw.githubusercontent.com/touchspeed/touchspeed.github.io/main/_posts/2024-07-11-rpm-update-wps/image-2.png)

使用rpm -ivh安装，提示冲突
![alt text](https://raw.githubusercontent.com/touchspeed/touchspeed.github.io/main/_posts/2024-07-11-rpm-update-wps/image.png)

使用rpm -Uvh，成功
![alt text](https://raw.githubusercontent.com/touchspeed/touchspeed.github.io/main/_posts/2024-07-11-rpm-update-wps/image-1.png)

## 总结

初次安装，使用rpm -ivh

更新应用，使用rpm -Uvh