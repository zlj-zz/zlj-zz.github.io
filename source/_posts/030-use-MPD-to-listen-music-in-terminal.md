---
title: 使用MPD在终端中听音乐
date: 2020-05-19
category: Linux
---

音乐播放器守护程序（MPD）是用于播放音乐的灵活而强大的服务器端应用程序。通过插件和库，它可以播放各种声音文件，并受其网络协议控制。为了与 mpd 进行交互，需要一个客户端程序。

![demo-image](https://img-blog.csdnimg.cn/20200519004703331.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4NDEwNDk0,size_16,color_FFFFFF,t_70)

**最常用的客户端应用程序是**：

> - **ncmpccpp**：几乎完全克隆了 ncmpc，其中包含一些用 C ++编写的新函数
> - **ncmpc**：mpd Ncurses 客户端
> - **mpc**：mpd 服务器的命令行用户界面

**install**

- 安装 **MPD** ，`sudo pacman -S mpd`
- 安装客户端，我使用 **ncmpc** ，`sudo pacman -S ncmpc`
- 配置文件您可以参考`/usr/share/doc/mpd/mpdconf.example`
- 如果需要配置文件可以从这里获取：[mpd.conf](https://github.com/zlj-zz/dotconfig/tree/master/mpd)

**mpc 的常用参数：**

`mpc` 查看当前播放歌曲的信息

`mpc add` 添加歌曲到播放列表

`mpc listall` 可以列出所有的歌曲

`mpc listall | mpc add` 可以把所有歌曲都添加到当前的播放列表

`mpc playlist` 查看当前播放列表

`mpc play` 播放

`mpc pause` 暂停

`mpc stop` 停止

`mpc next` 播放下一首

`mpc prev` 播放前一首

`mpc repeat on` 启用重复播放

`mpc random on` 启用随机播放

`mpc play 7` 播放列表中第 7 首

`mpc search filename` 可以按文件名查找

`mpc search artist` 可以按歌手查找

`mpc search title` 可以按歌曲名查找

`mpc volume +20`， `mpc volume -20` 音量加减，数值可以自己给

**问题**

- [x] **如果在配置文件中修改了端口**。

  答：连接时需要设置端口，例如：`ncmpc -p 6666`。而且我建议使用 `-C` 禁用颜色。

- [x] **要在登录时自动启动 MPD，请输入。**

  答：`systemctl --user enable mpd`。

- [x] **要在 tty 登录时自动启动 MPD。**

  答：将以下内容添加到〜/ .profile 或其他自动启动文件中。

  ```bash
  # MPD daemon start (if no other user instance exists)
    [ ! -s ~/.config/mpd/pid ] && mpd
  ```
