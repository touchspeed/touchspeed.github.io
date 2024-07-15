---
layout: post
title: 记一次 rm -rf 误删数据的恢复
date: 2024-07-15 16:44
category: Linux
author: Meve
tags: [Linux,Tips]
summary: 
---

## 前言

随便写的

之前把电脑装的Windows+Linux双系统，由于最近在搞系统备份and现在基本不怎么用win了，便想把win的系统盘数据整理下格式化当系统盘。<br>
由于文件比较多，有个文件夹和另一个无用文件夹名字相似，没仔细看就rm -rf了，事后发现删错了。<br>

## 恢复

这里我使用了testdisk, 还有一个extundelete但是不支持win的ntfs文件系统<br>

sudo dnf install testdisk<br>

先使用df -h找到需要恢复的挂载点<br>
![alt text](https://raw.githubusercontent.com/touchspeed/touchspeed.github.io/main/_posts/2024-07-15-restore-rm-data/image-1.png)<br>

sudo photorec /dev/nvme0n1p2<br>
![alt text](https://raw.githubusercontent.com/touchspeed/touchspeed.github.io/main/_posts/2024-07-15-restore-rm-data/image-1.png)<br>

![alt text](https://raw.githubusercontent.com/touchspeed/touchspeed.github.io/main/_posts/2024-07-15-restore-rm-data/image-2.png)<br>

![alt text](https://raw.githubusercontent.com/touchspeed/touchspeed.github.io/main/_posts/2024-07-15-restore-rm-data/image-3.png)<br>

![alt text](https://raw.githubusercontent.com/touchspeed/touchspeed.github.io/main/_posts/2024-07-15-restore-rm-data/image-4.png)<br>

这一步选择需要保存的位置<br>
![alt text](https://raw.githubusercontent.com/touchspeed/touchspeed.github.io/main/_posts/2024-07-15-restore-rm-data/image-5.png)<br>

等待恢复<br>
![alt text](https://raw.githubusercontent.com/touchspeed/touchspeed.github.io/main/_posts/2024-07-15-restore-rm-data/rec1.png)<br>

挂了一晚，结束了，退出<br>
![alt text](https://raw.githubusercontent.com/touchspeed/touchspeed.github.io/main/_posts/2024-07-15-restore-rm-data/rec2.png)<br>

恢复出来的文件结构是这样的，每个文件夹里面都有很多文件<br>
![alt text](https://raw.githubusercontent.com/touchspeed/touchspeed.github.io/main/_posts/2024-07-15-restore-rm-data/image-6.png)<br>

我决定先将所有文件夹按大小排序，看看比较大的文件夹都有什么，使用du -sh ./* | sort -h > rec.txt将排序的结果输出到rec.txt<br>

g rec.txt<br>
![alt text](https://raw.githubusercontent.com/touchspeed/touchspeed.github.io/main/_posts/2024-07-15-restore-rm-data/image-7.png)<br>

然后按照类别寻找重要的文件<br>

例如寻找doc文件<br>

find ./ -name "*.doc"<br>
![alt text](https://raw.githubusercontent.com/touchspeed/touchspeed.github.io/main/_posts/2024-07-15-restore-rm-data/image-8.png)<br>

将所有doc文件移动到../mmm文件夹<br>
find ./ -name "*.doc" -exec mv {} ../mmm \;<br>
