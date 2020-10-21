---
title: Linux remote connection, transmission, synchronization
date: 2020-10-20 23:32
category: Linux
---

# Use `rsync` to synchronization

一般我们使用 rsync 有两种认证方式，

    rsync-daemon方式，需要配置本地文件，灵活性一般
    ssh方式，直接远程连接传输，简单粗暴

记录一下相关命令参数，以防忘记。

同步需求

本机：本机用户 bear，/home/bear/mydir 目录

远程服务器：root@114.215.183.37 ，USE 用户 bear 下 /home/bear/workdir 目录

命令使用

## 从本地同步到远程

默认 SSH 端口

```shell
rsync -avz -e ssh /home/bear/mydir root@114.215.183.37:/home/bear/workdir
```

指定 SSH 端口

```shell
rsync -avz -e 'ssh -p 5555' /home/bear/mydir root@114.215.183.37:/home/bear/workdir
```

## 从远程同步到本地

默认 SSH 端口

```shell
rsync -avzP -e ssh root@114.215.183.37:/home/bear/workdir /home/bear/mydir
```

指定 SSH 端口

```shell
rsync -avzP -e 'ssh -p 5555' root@114.215.183.37:/home/bear/workdir /home/bear/mydir
```

---

# Linux 使用 sshfs 挂载远程目录到本地

挂载远程目录的方式很多，这里把 sshfs 记录一下备忘。

安装 sshfs

在 Ubuntu 下，只需要使用

```shell
\$ sudo apt-get install sshfs
```

一共才 140kB

挂载远程目录到本地

```shell
\$ sshfs user@hostIP:path MountDir
```

例如

```shell
\$ sshfs Suzzz@192.168.103.111:/home/Suzzz ~/Tmp/
```

这样就把远程目录 `/home/Suzzz` 挂载到了本地的 `~/Tmp` 下

这样的挂载，在文件管理器中也是可见的。

现在可以在终端或者图形界面来访问这个目录了

卸载

```shell
fusermount -u MountDir
```
