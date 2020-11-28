---
title: linux下virtualbox使用
date: 2020-05-27
category: Linux
tags:
---

这里以自己使用的系统为案例 [MANJARO]

**VirtualBox** 是一种流行的应用程序，它允许在现有操作系统（称为 Host）中安装和运行其他操作系统（称为 Guests）。

# 安装

安装命令：

要安装 VirtualBox，您需要安装软件包 `virtualbox` 和 `linux*-virtualbox-host-modules`。后者必须与您正在运行的内核版本匹配。要列出已安装的内核，请使用 `mhwd`:

```bash
~> mhwd-kernel -li
Currently running: 5.4.0-1-MANJARO (linux54)
The following kernels are installed in your system:
   * linux54
```

请在终端中输入以下命令进行安装：

```bash
sudo pacman -Syu virtualbox linux54-virtualbox-host-modules
```

安装完成后，有必要将 VirtualBox 模块添加到您的内核中。简单的方法是简单地重新启动系统。若要立即使用 VirtualBox，输入以下指令：

```bash
sudo vboxreload
```

# 创建虚拟机

![](https://img-blog.csdnimg.cn/20200527230608350.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4NDEwNDk0,size_16,color_FFFFFF,t_70#pic_center)

![](https://img-blog.csdnimg.cn/20200527231003133.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4NDEwNDk0,size_16,color_FFFFFF,t_70#pic_center)

![](https://img-blog.csdnimg.cn/2020052723101841.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4NDEwNDk0,size_16,color_FFFFFF,t_70#pic_center)

一路 `next`

![](https://img-blog.csdnimg.cn/20200527231406775.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4NDEwNDk0,size_16,color_FFFFFF,t_70#pic_center)

![](https://img-blog.csdnimg.cn/20200527231751967.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4NDEwNDk0,size_16,color_FFFFFF,t_70#pic_center)

然后，点击 `start` 启动虚拟机。

# 共享文件夹和粘贴板，双向托放

### 共享粘贴板

![在这里插入图片描述](https://img-blog.csdnimg.cn/2020052723345798.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4NDEwNDk0,size_16,color_FFFFFF,t_70#pic_center)

### 双向托放

![](https://img-blog.csdnimg.cn/20200527233442900.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4NDEwNDk0,size_16,color_FFFFFF,t_70#pic_center)

### 共享文件夹

如果你虚拟机中是 linux 系统，直接在设置中设置共享文件夹路径即可。

![](https://img-blog.csdnimg.cn/20200527233719542.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4NDEwNDk0,size_16,color_FFFFFF,t_70#pic_center)

如果你虚拟机中是 windows，那么在设置好共享文件夹后还需要一些额外的操作。

首先你需要安装增强功能。

![](https://img-blog.csdnimg.cn/20200527234655728.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4NDEwNDk0,size_16,color_FFFFFF,t_70#pic_center)

安装完成后会出现一个驱动，点击安装该驱动。安装完成后重启就可以看到你的共享文件夹了。

![](https://img-blog.csdnimg.cn/20200527234751103.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4NDEwNDk0,size_16,color_FFFFFF,t_70#pic_center)
