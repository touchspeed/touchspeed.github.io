---
layout: post
title: IC EDA on Windows using WSL
date: 2024-05-07 16:00
category: Linux
author: Meve
tags: [EDA,Linux,WSL]
summary: 
---

## 前言

本文记录在wsl上使用eda遇到的一些问题，这是我的wsl版本信息:<br>
![alt text](https://raw.githubusercontent.com/touchspeed/touchspeed.github.io/main/_posts/2024-05-07-ic-eda-on-windows-using-wsl/wsl.png)<br>
基于ubuntu20.04 wsl，安装的synopsys eda大部分版本都是2018。<br>
如果是ubuntu22.04会遇到一些棘手的问题，难以解决。

## 痛点

最最最难受的一点，wsl每次电脑重启mac会变化，导致synopsys、matlab之类的license失效。<br>
![alt text](https://raw.githubusercontent.com/touchspeed/touchspeed.github.io/main/_posts/2024-05-07-ic-eda-on-windows-using-wsl/lm1.png)<br>
![alt text](https://raw.githubusercontent.com/touchspeed/touchspeed.github.io/main/_posts/2024-05-07-ic-eda-on-windows-using-wsl/lm2.png)<br>
下面给出解决方法：<br>
在Windows中的C:\Users\ [your_username]目录下创建一个.wslconfig文件，然后在文件中写入如下内容

``` json
[experimental]
autoMemoryReclaim=gradual
networkingMode=mirrored
dnsTunneling=true
firewall=true
autoProxy=true
```
mirror模式下可以发现ifconfig的mac就是真实mac。<br>
[参考1](https://unix.stackexchange.com/questions/772303/machine-mac-address-with-ubuntu-on-top-of-wsl2)<br>
[参考2](https://github.com/microsoft/WSL/issues/5352)<br>
[参考3](https://github.com/microsoft/WSL/issues/5291)<br>
[参考4](https://github.com/microsoft/WSL/issues/10753)<br>
本来是为了解决wsl不走代理的问题，结果发现这样可以固定eda license mac。<br>

## 开始

使用synopsys installer，setup.sh，缺少libXss.so.1，如图:<br>
![alt text](https://raw.githubusercontent.com/touchspeed/touchspeed.github.io/main/_posts/2024-05-07-ic-eda-on-windows-using-wsl/2.png)<br>
[参考此文章](https://www.cnblogs.com/taitai139/p/14046962.html)，安装libXss1:<br>
![alt text](https://raw.githubusercontent.com/touchspeed/touchspeed.github.io/main/_posts/2024-05-07-ic-eda-on-windows-using-wsl/3.png)<br>

lmgrd提示no such file，sudo apt-get install lsb-core即可解决<br>
![alt text](https://raw.githubusercontent.com/touchspeed/touchspeed.github.io/main/_posts/2024-05-07-ic-eda-on-windows-using-wsl/4.png)<br>
![alt text](https://raw.githubusercontent.com/touchspeed/touchspeed.github.io/main/_posts/2024-05-07-ic-eda-on-windows-using-wsl/lsb.png)<br>
无法创建.flexlm问题:<br>
![alt text](https://raw.githubusercontent.com/touchspeed/touchspeed.github.io/main/_posts/2024-05-07-ic-eda-on-windows-using-wsl/5.png)<br>
解决:<br>
![alt text](https://raw.githubusercontent.com/touchspeed/touchspeed.github.io/main/_posts/2024-05-07-ic-eda-on-windows-using-wsl/6.png)<br>

再次lmgrd激活提示端口占用，lmgrd failed to open the tcp port，ps找出进程编号kill掉，再等一段时间就可以再次激活license：<br>
![alt text](https://raw.githubusercontent.com/touchspeed/touchspeed.github.io/main/_posts/2024-05-07-ic-eda-on-windows-using-wsl/7.png)<br>

dvt正常:<br>
![alt text](https://raw.githubusercontent.com/touchspeed/touchspeed.github.io/main/_posts/2024-05-07-ic-eda-on-windows-using-wsl/dvt.png)<br>

euclide权限问题，提示不能写权限运行，或run with 'private_install'，这是目录权限问题导致:<br>
![alt text](https://raw.githubusercontent.com/touchspeed/touchspeed.github.io/main/_posts/2024-05-07-ic-eda-on-windows-using-wsl/euclide1.png)<br>
改下目录权限即可，用户组改为root，sudo chown root xxxx，如图:<br>
![alt text](https://raw.githubusercontent.com/touchspeed/touchspeed.github.io/main/_posts/2024-05-07-ic-eda-on-windows-using-wsl/euclide2.png)<br>
euclide cannot open display问题<br>
![alt text](https://raw.githubusercontent.com/touchspeed/touchspeed.github.io/main/_posts/2024-05-07-ic-eda-on-windows-using-wsl/euclide3.png)<br>
进入euclide的eclipse文件夹，sudo vim euclide<br>
![alt text](https://raw.githubusercontent.com/touchspeed/touchspeed.github.io/main/_posts/2024-05-07-ic-eda-on-windows-using-wsl/euclide4.png)<br>
如图注释掉<br>
![alt text](https://raw.githubusercontent.com/touchspeed/touchspeed.github.io/main/_posts/2024-05-07-ic-eda-on-windows-using-wsl/euclide5.png)<br>
可以运行:<br>
![alt text](https://raw.githubusercontent.com/touchspeed/touchspeed.github.io/main/_posts/2024-05-07-ic-eda-on-windows-using-wsl/euclide6.png)<br>
新的问题，暂时无法解决<br>
![alt text](https://raw.githubusercontent.com/touchspeed/touchspeed.github.io/main/_posts/2024-05-07-ic-eda-on-windows-using-wsl/euclide7.png)<br>

verdi提示syntax error<br>
![alt text](https://raw.githubusercontent.com/touchspeed/touchspeed.github.io/main/_posts/2024-05-07-ic-eda-on-windows-using-wsl/verdi1.png)<br>
sudo dpkg-reconfigure dash，选择no，遇到新的问题<br>
![alt text](https://raw.githubusercontent.com/touchspeed/touchspeed.github.io/main/_posts/2024-05-07-ic-eda-on-windows-using-wsl/verdi2.png)<br>
尝试安装libXmu6找不到<br>
![alt text](https://raw.githubusercontent.com/touchspeed/touchspeed.github.io/main/_posts/2024-05-07-ic-eda-on-windows-using-wsl/verdi3.png)<br>
search一下，发现了吗，so库是libXmu6，需要安装的是libxmu6，这...<br>
![alt text](https://raw.githubusercontent.com/touchspeed/touchspeed.github.io/main/_posts/2024-05-07-ic-eda-on-windows-using-wsl/verdi4.png)<br>
新的依赖<br>
![alt text](https://raw.githubusercontent.com/touchspeed/touchspeed.github.io/main/_posts/2024-05-07-ic-eda-on-windows-using-wsl/verdi5.png)<br>

``` bash
sudo add-apt-repository ppa:linuxuprising/libpng12
sudo apt update
sudo apt install libpng12-0
```

nlint，如图安装依赖<br>
![alt text](https://raw.githubusercontent.com/touchspeed/touchspeed.github.io/main/_posts/2024-05-07-ic-eda-on-windows-using-wsl/nlint.png)<br>
nlint -gui，如图安装依赖<br>
![alt text](https://raw.githubusercontent.com/touchspeed/touchspeed.github.io/main/_posts/2024-05-07-ic-eda-on-windows-using-wsl/nlint1.png)<br>
成功<br>
![alt text](https://raw.githubusercontent.com/touchspeed/touchspeed.github.io/main/_posts/2024-05-07-ic-eda-on-windows-using-wsl/nlint2.png)<br>

spyglass正常<br>
![alt text](https://raw.githubusercontent.com/touchspeed/touchspeed.github.io/main/_posts/2024-05-07-ic-eda-on-windows-using-wsl/sg.png)<br>

formality，安装csh<br>
![alt text](https://raw.githubusercontent.com/touchspeed/touchspeed.github.io/main/_posts/2024-05-07-ic-eda-on-windows-using-wsl/fm.png)<br>
新的问题，如图安装依赖libgl1<br>
![alt text](https://raw.githubusercontent.com/touchspeed/touchspeed.github.io/main/_posts/2024-05-07-ic-eda-on-windows-using-wsl/fm1.png)<br>
成功<br>
![alt text](https://raw.githubusercontent.com/touchspeed/touchspeed.github.io/main/_posts/2024-05-07-ic-eda-on-windows-using-wsl/fm2.png)<br>
一个警告<br>
![alt text](https://raw.githubusercontent.com/touchspeed/touchspeed.github.io/main/_posts/2024-05-07-ic-eda-on-windows-using-wsl/fm3.png)<br>
在~/.bashrc中添加，这个还能解决formality、dc交互shell中的上下键乱码<br>
![alt text](https://raw.githubusercontent.com/touchspeed/touchspeed.github.io/main/_posts/2024-05-07-ic-eda-on-windows-using-wsl/fm4.png)<br>

tmax tetramax<br>
![alt text](https://raw.githubusercontent.com/touchspeed/touchspeed.github.io/main/_posts/2024-05-07-ic-eda-on-windows-using-wsl/tmax.png)<br>
解决方法<br>
![alt text](https://raw.githubusercontent.com/touchspeed/touchspeed.github.io/main/_posts/2024-05-07-ic-eda-on-windows-using-wsl/tmax1.png)<br>
![alt text](https://raw.githubusercontent.com/touchspeed/touchspeed.github.io/main/_posts/2024-05-07-ic-eda-on-windows-using-wsl/tmax2.png)<br>
建立链接<br>
![alt text](https://raw.githubusercontent.com/touchspeed/touchspeed.github.io/main/_posts/2024-05-07-ic-eda-on-windows-using-wsl/tmax3.png)<br>
安装libmng2<br>
![alt text](https://raw.githubusercontent.com/touchspeed/touchspeed.github.io/main/_posts/2024-05-07-ic-eda-on-windows-using-wsl/tmax4.png)<br>
建立链接<br>
![alt text](https://raw.githubusercontent.com/touchspeed/touchspeed.github.io/main/_posts/2024-05-07-ic-eda-on-windows-using-wsl/tmax5.png)<br>
成功<br>
![alt text](https://raw.githubusercontent.com/touchspeed/touchspeed.github.io/main/_posts/2024-05-07-ic-eda-on-windows-using-wsl/tmax6.png)<br>

lc_shell lib compiler，安装libpulse0<br>
![alt text](https://raw.githubusercontent.com/touchspeed/touchspeed.github.io/main/_posts/2024-05-07-ic-eda-on-windows-using-wsl/lc.png)<br>

icc_shell ic compiler，遇到glibc问题<br>
![alt text](https://raw.githubusercontent.com/touchspeed/touchspeed.github.io/main/_posts/2024-05-07-ic-eda-on-windows-using-wsl/icc.png)<br>
网上的解决方法，我没有尝试，从ubuntu22.04退回20.04没有此问题<br>
![alt text](https://raw.githubusercontent.com/touchspeed/touchspeed.github.io/main/_posts/2024-05-07-ic-eda-on-windows-using-wsl/icc1.png)<br>

tessent -shell<br>

``` bash
sudo apt install libgtk2.0-0
sudo apt install libpangoxft-1.0-0
```

calibre -gui，invalid operating system<br>
![alt text](https://raw.githubusercontent.com/touchspeed/touchspeed.github.io/main/_posts/2024-05-07-ic-eda-on-windows-using-wsl/calibre.png)<br>
/etc/redhat-release存有以下系统版本内容就不会再报<br>
![alt text](https://raw.githubusercontent.com/touchspeed/touchspeed.github.io/main/_posts/2024-05-07-ic-eda-on-windows-using-wsl/calibre1.png)<br>
新建文件填入上面的内容，并设置权限<br>
![alt text](https://raw.githubusercontent.com/touchspeed/touchspeed.github.io/main/_posts/2024-05-07-ic-eda-on-windows-using-wsl/calibre2.png)<br>

Virtuoso，过程比较乱，没有整理<br>
![alt text](https://raw.githubusercontent.com/touchspeed/touchspeed.github.io/main/_posts/2024-05-07-ic-eda-on-windows-using-wsl/image.png)<br>
![alt text](https://raw.githubusercontent.com/touchspeed/touchspeed.github.io/main/_posts/2024-05-07-ic-eda-on-windows-using-wsl/image-1.png)<br>
![alt text](https://raw.githubusercontent.com/touchspeed/touchspeed.github.io/main/_posts/2024-05-07-ic-eda-on-windows-using-wsl/image-2.png)<br>
![alt text](https://raw.githubusercontent.com/touchspeed/touchspeed.github.io/main/_posts/2024-05-07-ic-eda-on-windows-using-wsl/image-3.png)<br>
![alt text](https://raw.githubusercontent.com/touchspeed/touchspeed.github.io/main/_posts/2024-05-07-ic-eda-on-windows-using-wsl/image-4.png)<br>
![alt text](https://raw.githubusercontent.com/touchspeed/touchspeed.github.io/main/_posts/2024-05-07-ic-eda-on-windows-using-wsl/image-5.png)<br>
![alt text](https://raw.githubusercontent.com/touchspeed/touchspeed.github.io/main/_posts/2024-05-07-ic-eda-on-windows-using-wsl/image-6.png)<br>
![alt text](https://raw.githubusercontent.com/touchspeed/touchspeed.github.io/main/_posts/2024-05-07-ic-eda-on-windows-using-wsl/image-7.png)<br>
![alt text](https://raw.githubusercontent.com/touchspeed/touchspeed.github.io/main/_posts/2024-05-07-ic-eda-on-windows-using-wsl/image-8.png)<br>
![alt text](https://raw.githubusercontent.com/touchspeed/touchspeed.github.io/main/_posts/2024-05-07-ic-eda-on-windows-using-wsl/image-9.png)<br>
![alt text](https://raw.githubusercontent.com/touchspeed/touchspeed.github.io/main/_posts/2024-05-07-ic-eda-on-windows-using-wsl/image-10.png)<br>
集成calibre，需要~/.cdsinit<br>
![alt text](https://raw.githubusercontent.com/touchspeed/touchspeed.github.io/main/_posts/2024-05-07-ic-eda-on-windows-using-wsl/image-11.png)<br>
新建tmp文件夹<br>
![alt text](https://raw.githubusercontent.com/touchspeed/touchspeed.github.io/main/_posts/2024-05-07-ic-eda-on-windows-using-wsl/image-12.png)<br>
![alt text](https://raw.githubusercontent.com/touchspeed/touchspeed.github.io/main/_posts/2024-05-07-ic-eda-on-windows-using-wsl/image-13.png)<br>
![alt text](https://raw.githubusercontent.com/touchspeed/touchspeed.github.io/main/_posts/2024-05-07-ic-eda-on-windows-using-wsl/image-14.png)<br>
![alt text](https://raw.githubusercontent.com/touchspeed/touchspeed.github.io/main/_posts/2024-05-07-ic-eda-on-windows-using-wsl/image-15.png)<br>
安装字体<br>
![alt text](https://raw.githubusercontent.com/touchspeed/touchspeed.github.io/main/_posts/2024-05-07-ic-eda-on-windows-using-wsl/image-16.png)<br>


## 最后
折腾了很久，最后还是用回Linux实机，顺便把我用了几年的kubuntu本子换成了Rocky Linux，虽然ubuntu系列在娱乐方面有优势，但是最近我遇到了一个关于vivado的bug，让我直接选择奔向rhel系。<br>
环境迁移倒是很简单，备份一下home，再把需要的文件解压到新系统就可以，EDA不需要安装可以直接运行遇到依赖问题修复下就可以，repoquery --nvr --whatprovides真的很方便。<br>
而nvidia驱动方面，就没有ubuntu那么方便，折腾半天才把驱动跑起来:D