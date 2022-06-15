---
title: pigit 快速使用 git
date: 2021-08-30
category:
---

相信对于每一个开发人员来时，git 绝对不会陌生。Git 是一个免费的、开源的分布式版本控制系统，旨在以快速高效的方式处理从小型到大型的项目。

当我们在使用 `git status` `git branch` 这些命令操作 git 时，有没有觉得不够快。这个是否我么可以借助一些第三方的工具来帮助我们快速、方便的使用 git。

Git 的图形化工具不少，我作为一个 terminal 爱好者，在这里向大家介绍一款自己开发的 terminal 下的 git 辅助工具 -- **PIGIT**

该工具选择使用 python 开发的原因是因为大部分的类 uinx 系统都有预装 python，python 环境本身的安装也十分简单。

项目地址： [https://github.com/zlj-zz/pigit](https://github.com/zlj-zz/pigit)

pigit 是一款基于 Python 编写的 git 辅助工具。可以通过 pip 快速的安装它：

```shell
pip install pigit -U
```

当我们安装完后，在 terminal 中输入 `pigit` 就可以开始使用它。

pigit 预先定义的大量的 git 操作，我们可以直接使用 pigit 提供的短命令触发该操作。 你可使用 `pigit -s` 来查看它提供了哪些预定义命令：

```
These are short commands that can replace git operations:
    b        lists, creates, renames, and deletes branches.
             git branch
    bc       creates a new branch.
             git checkout -b
    bl       lists branches and their commits.
             git branch -vv
    bL       lists local and remote branches and their commits.
             git branch --all -vv
    bs       lists branches and their commits with ancestry graphs.
             git show-branch
    bS       lists local and remote branches and their commits with ancestry graphs.
             git show-branch --all
    bm       renames a branch.
             git branch --move
    bM       renames a branch even if the new branch name already exists.
             git branch --move --force
    bd       delete a local branch by name.
             git branch -d
    c        records changes to the repository.
             git commit --verbose
......
```

pigit 还提供了一个简单的命令行交互，来快速操作 git 的工作树，避免 `git add file1 file2 ...` 这样的操作。

![gif demo](https://img-blog.csdnimg.cn/86ebb5be34b1443e95f2a4b597a7f3b4.gif#pic_center)
除此之外， pigit 还提供了一些与项目有关的功能。 如：代码统计、创建 `.gitignore` 模版、查看项目信息....

![gif demo](https://img-blog.csdnimg.cn/23fb83dfabc64f0eb654050f446369f6.gif#pic_center)
更多的用法可以到项目的 github 中查看。
