---
layout: post
title: rocky 系统备份
date: 2024-07-13 01:53
category: Linux
author: Meve
tags: [Linux,Tips]
summary: 
---


sudo lvscan

![alt text](https://raw.githubusercontent.com/touchspeed/touchspeed.github.io/main/_posts/2024-07-13-rocky-system-backup/image.png)

sudo lvdisplay /dev/rl/root

![alt text](https://raw.githubusercontent.com/touchspeed/touchspeed.github.io/main/_posts/2024-07-13-rocky-system-backup/image-1.png)

df -h /dev/rl/root

![alt text](https://raw.githubusercontent.com/touchspeed/touchspeed.github.io/main/_posts/2024-07-13-rocky-system-backup/image-2.png)

sudo lvcreate --size 20G --snapshot --name root_snap07132024 /dev/rl/root

sudo dnf install timeshift
