---
layout: post
title: rocky 系统备份
date: 2024-07-13 01:53
category: Linux
author: Meve
tags: [Linux,Tips]
summary: 
---

## LVM

Linux 并不稳定，在我觉得甚至安装、卸载一些软件、升级依赖就会导致系统崩掉，所以安全措施很重要

本来打算用 lvm 开启系统快照

sudo lvscan

![alt text](https://raw.githubusercontent.com/touchspeed/touchspeed.github.io/main/_posts/2024-07-13-rocky-system-backup/image.png)

sudo lvdisplay /dev/rl/root

![alt text](https://raw.githubusercontent.com/touchspeed/touchspeed.github.io/main/_posts/2024-07-13-rocky-system-backup/image-1.png)

df -h /dev/rl/root

![alt text](https://raw.githubusercontent.com/touchspeed/touchspeed.github.io/main/_posts/2024-07-13-rocky-system-backup/image-2.png)

sudo lvcreate --size 20G --snapshot --name root_snap07132024 /dev/rl/root

执行发现硬盘剩余未分配空间太小，无法创建快照卷

## Timeshift

最后选择用 Timeshift

sudo dnf install timeshift
