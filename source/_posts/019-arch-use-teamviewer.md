---
title: arch department use teamviewer
date: 2020-05-15 01:01:06
category: Linux
tags:
---

**Teamviewer** is a simple and fast solution that can be used for remote control of application in the background of any firewall and NAT proxy, desktop sharing and file transfer.

First, we should install teamviewer.

```shell
sudo pacman -S teamviewer
```

Then we should enable the server of teamviewer.

```shell
teamviewer --daemon start
```

And you can set it to start automatically.

```shell
teamviewer --daemon enable
```

Next you can start using.

If you don't have **ID**, you may should add this to `/opt/teamviewer/config/global.conf`

```bash
[int32] EulaAccepted = 1
[int32] EulaAcceptedRevision = 6
```

Generally speaking, this is no problem.

![demo](https://img-blog.csdnimg.cn/20200515011802279.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4NDEwNDk0,size_16,color_FFFFFF,t_70)
