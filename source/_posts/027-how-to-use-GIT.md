---
title: 如何流畅丝滑的使用git
date: 2020-09-07
category: Linux
---

@[TOC](这里写目录标题)

# git 使用

<kbd>git init</kbd> 可以将一个文件夹初始化为一个 git 目录，会生成一个`.git`的隐藏目录。

<kbd>git remote add \<url\></kbd> 可以帮助我们连接到一个远程的仓库。

<kbd>git clone [-b branch-name] \<url\> </kbd> 可以克隆一个远程仓库到本地，`-b`参数可以指定分支。

<kbd>git status</kbd>查看工作树状态。

<kbd>git add \<file\></kbd>可以添加文件的变化到暂存区，可以用`.`表示添加所有变化。

> `-u`参数用来添加被追踪的文件，即新建文件不会被添加。

<kbd>git diff \<file\></kbd>查看文件的改变。

<kbd>git commit</kbd>用来提交文件，回车后自动打开编辑器让你编辑提交信息。

> 可以通过`-m <msg>`在命令行添加提交信息。
> 通过`-amend` 参数将本次提交添加到上一次提交中，加上`--no-edit`参数可以不同编辑提交信息。

<kbd>git push</kbd>将本地分支推送到远程，`-f`参数可以强制推送。

<kbd>git fetch</kbd>将远程最新的内容拉到本地。

<kbd>git merge [branch-name]</kbd>合并分支，默认合并远程分支；可以将指定分支合并到当前分支。

<kbd>git pull</kbd>等于`git fetch`+`git merge`

<kbd>git branch [branch-name]</kbd>查看分支或者创建新分支。

<kbd>git checkout</kbd>切换分支。

> 使用`-b`参数创建并切换分支。
> 合并冲突时，`--ours|--theirs [file]`选择使用哪个分支。

<kbd>git log</kbd> 查看**commit**提交信息日志。

> `-graph`以图的形式显示。
> `-all`查看全部分支。
> `-oneline`条件信息只显示一行。

<kbd>git reset</kbd> 复位操作，可以用于`add`, `commit`

> `git reset HEAD [file]` 撤销添加操作。
> `git reset \<HEAD~n|版本号\>` 退回上 n 个版本或指定版本
> `--hard` 清楚工作区
> `--merged` 回滚时不清楚不要的版本工作区
> `--soft` 仅撤回提交，工作区内容不变
> `--keep` 清楚提交，保持工作区不变

---

# git 设置

使用`HTTP`协议时，可以使用<kbd>git config credential.helper store</kbd>来记住用户名和密码。

可以使用<kbd>git config --global core.editor /usr/bin/vim</kbd>指定提交信息的编辑器。

---

# git commit 信息规范

### why

1. 提供更多的历史信息，方便快速浏览。
2. 可以过滤某些 commit（比如文档改动），便于快速查找信息。
3. 可以直接从 commit 生成 Change log。

### AngularJS 规范

> 它是由 Google 推出的一套提交消息规范标准，也是目前使
> 用范围最广的规范。有一套合理的手册也较为系统化；并且还有配套的工具可以供我们使用。

![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pbWcyMDE4LmNuYmxvZ3MuY29tL2Jsb2cvMzI4NTk5LzIwMTkwMS8zMjg1OTktMjAxOTAxMjMxMDQxMDExNjAtODQ1MzY4MTYwLnBuZw?x-oss-process=image/format,png#pic_center)

##### 格式

```bash
<type>(<scope>): <subject>
<BLANK LINE>
<body>
<BLANK LINE>
<footer>
```

##### 要求

- 消息只占用一行，任何行都不能超过 100 个字符
- 允许使用 GitHub 以及各种 Git 工具阅读消息
- 提交消息由页眉、正文和页脚组成，由空行分隔
