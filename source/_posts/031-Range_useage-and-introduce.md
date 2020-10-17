---
title: 控制台文件管理器-Ranger
date: 2020-02-23
category: Linux
---

今天给大家介绍一款特别的软件。在终端上，我们可以通过 `ls` 来查看文件，同过 `ls` 命令我们只能看到当前目录下的文件。那么有没有办法可以在终端中清晰的看到目录的层级关系，方便的查看文件呢？
如果你有这样的想法或需求，那么 `Ranger` 绝对是你想要的。`Ranger` 是具有 VI 键绑定的控制台文件管理器。它提供了一个简约而美观的 `curses` 界面，并具有目录层次结构的视图。它附带了`rifle`（一个文件启动器），该文件启动器擅长自动找出用于哪种文件类型的程序。看看效果
![01](https://img-blog.csdnimg.cn/20200225174158198.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4NDEwNDk0,size_16,color_FFFFFF,t_70)

## 安装

如何安装 `Ranger` 呢？首先 `Ranger`是依赖 Python 的，所以你的电脑里需要有 Python（`>=2.6或>=3.1`）
我们可以通 PyPI 的方式安装`Ranger`： `sudo pip install ranger`
安装完成后打开终端输入 `ranger` 回车，我们就可以开启 `Ranger` 之旅了
<img src="https://img-blog.csdnimg.cn/20200225174231650.gif" width="100%">

## 入门

启动 `Ranger` 后，你会看到三个面板

- 中间一个包含当前目录的内容，
- 左侧显示父目录，突出显示当前目录，
- 右边的提供各种预览。

您可以使用箭头键或 `h` `j` `k` `l` 进行导航，`Enter` 打开文件或 `q` 退出。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200225174413155.gif)
当前文件的属性可以在底部看到，而当前目录路径可以在顶部看到。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200225174348856.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4NDEwNDk0,size_16,color_FFFFFF,t_70)

## 进阶

如果你熟悉 `vim`，那么使用 `ranger` 将十分容易。`ranger` 借鉴了许多 `vim` 语法。例如：`yy`复制文件（在 `vim` 中复制一行），`pp` 粘贴，`dd` 剪切文件。还可以使用 `:`（冒号）调用的命令行中调用更复杂的函数。

要打开文件并让 `Ranger` 选择要使用的程序（通过`rifle`）打开 ，只需按 `Enter`。要选择自己，请按 `r`，然后从列表中选择或键入要使用的程序。你也可以使用命令 `:open_with`。
<img src="https://img-blog.csdnimg.cn/20200225174441939.gif" width="100%">

如果你一次要在一个以上的目录中工作，则可以为每个要快速访问的目录创建选项卡。选项卡已编号。要切换到第 N 个标签，请按 `Alt-N`，其中 N 是标签编号。如果这样的标签还不存在，它将被创建。要关闭选项卡，请按`q`（如果关闭最后一个选项卡，则整个 ranger 过程将关闭）。

<img src="https://img-blog.csdnimg.cn/20200225174616637.gif" width="100%">

**Ranger**: [https://ranger.github.io/index.html](https://ranger.github.io/index.html)
