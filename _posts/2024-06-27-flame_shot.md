---
layout: post
title: flameshot，平替snipaste
date: 2024-06-28 19:20
category: 实用软件
author: Meve
tags: [Linux]
summary: 
---

### 前言

snipaste是我在win上经常用到的一个截图软件，操作方便，贴图功能也很便利。 <br>
由于现在我更多使用linux，便想寻找一个替代品，而flameshot，则是目前我认为linux上最好用的截图软件。 <br>
机器系统为Rocky Linux 8.10。 <br>

### 获取

从[这里](https://github.com/flameshot-org/flameshot/releases/tag/v12.1.0)下载，最新版本停留在12.1.0，这里我选择下载appimage，可执行单文件，当然也可以下载rpm包之后安装到系统(sudo rpm -ivh xxxx.rpm)。 <br>
![flameshot_release](/assets/img/get_flameshot/flameshot_release.png)

### 统一管理便携应用

将appimage移动到/home/tools/Utilities/ <br>
mv ./Flameshot-12.1.0.x86_64.AppImage /home/tools/Utilities/ <br>

进入/home/tools/Utilities/文件夹，chmod +x Flameshot-12.1.0.x86_64.AppImage给一下执行权限 <br>
![utilities](/assets/img/get_flameshot/utilities.png)

/home/tools/里面都是我存放的一些应用 <br>
![/home/tools/](/assets/img/get_flameshot/home_tools.png)

进入/home/tools/bin/ <br>
![soft_links](/assets/img/get_flameshot/soft_links.png)

将应用的可执行文件软链接到/home/tools/bin/ <br>
ln -snf ../Utilities/Flameshot-12.1.0.x86_64.AppImage ./flameshot <br>

在.bashrc中将可执行文件目录导入PATH <br>
![tool_path](/assets/img/get_flameshot/tool_path.png)

打开一个新的终端，输入flameshot即可打开软件。<br>

### 优点

无需安装，开箱即用，也方便了系统迁移，只要将tools文件夹复制到新机器的/home/下，再在.bashrc中将可执行文件目录导入PATH，就能使用这些软件，当然appimage在不同系统可能还是会遇到某些依赖问题。 <br>

### 配置快捷键

在使用软件的时候，我准备将ctrl+alt+s作为截屏键，flameshot gui是截图命令 <br>
![alt text](/assets/img/get_flameshot/shortcut.png)

结果发现left alt和left super是反着的，这不能忍(虽然可以调换键帽hh)，我们直接打开gnome tweaks，没有的话可以sudo yum install gnome-tweaks，然后如下图设置可以将按键换回来。 <br>
![gnome_tweaks](/assets/img/get_flameshot/gnome_tweaks.png)

## 结束
