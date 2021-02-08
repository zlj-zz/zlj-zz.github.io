---
title: Raspberry Pi 构建恢复分区，自动恢复系统
date: 2021-02-04 00:00
category: Linux
---

首先要将树莓派的磁盘挂载到另一台电脑上，以便后续的操作。

# 创建分区

第一步就是要创建一个新的分区用于恢复系统。你可以使用 `fdisk` 命令去创建。也可以使用一些图形化的工具。

> **Note**: 记得分区时留下足够的空间去安装系统和存放备份镜像。

这里我是在 **Ubuntu** 下使用 `gparted` 工具来分去的。

```bash
sudo apt install gparted
```

![GParted](https://img-blog.csdnimg.cn/20210204165914976.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4NDEwNDk0,size_16,color_FFFFFF,t_70#pic_center)
分区完了之后我们用 `sudo lsblk --fs` 看一看结果。

```
NAME           FSTYPE LABEL      UUID                                 MOUNTPOINT
sdd
├─sdd2         ext4   rootfs     80571af6-21c9-48a0-9df5-cffb60cf79af /media/ubuntu/rootfs
├─sdd3         ext4   recoveryfs a7adb26a-8b87-4729-99c8-9f5ac069d51e /media/ubuntu/recoveryfs
└─sdd1         vfat   boot       6341-C9E5                            /media/ubuntu/boot
sdb
├─sdb4         ext4              3eb0c4d3-d782-44f8-b136-be73e8aa1ac4 /media/ubuntu/3eb0c4d3-d782-44f8-b136-be73e8aa1ac4
├─sdb2         ntfs              5EE0C595E0C57435                     /media/ubuntu/5EE0C595E0C57435
├─sdb5         swap              652b8cfb-fdfd-49c2-8041-3643892aa97d
│ └─cryptswap1 swap              e68424e3-2799-4f72-8754-dacd7ad1a61c [SWAP]
├─sdb3         vfat              1441-F786
└─sdb1
sda
├─sda4         ext4              aab4fc34-f6f3-43b4-a7c2-7d70f189d0da /
├─sda2         vfat              0AB2-A61D                            /boot/efi
├─sda5         ntfs              103ED30C3ED2EA2C
├─sda3
└─sda1         ntfs   恢复       320AB1060AB0C85D
```

这里 `sdd` 就是我们的树莓派硬盘：

- `sdd1`: boot 引导分区
- `sdd2`: 正常系统分区
- `sdd2`: 恢复系统分区

# 安装镜像

这里推荐**Raspberry Pi OS Lite**，比较小。下载地址：[https://www.raspberrypi.org/software/operating-systems/](https://www.raspberrypi.org/software/operating-systems/)

这里你是不能直接用 `dd` 去安装的。因为你的目标是一个分区，而镜像中可能包含了多个分区。这时就需要 `losetup` 命令。

#### 查看镜像信息

`sudo fdisk -lu <xxx.img>` 用来查看镜像的信息。

```
ubuntu@ubuntu-MRC-WX0:~$ sudo fdisk -lu 2021-01-11-raspios-buster-armhf-lite.img
Disk 2021-01-11-raspios-buster-armhf-lite.img: 1.8 GiB, 1862270976 bytes, 3637248 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0xe8af6eb2

Device                                    Boot  Start     End Sectors  Size Id Type
2021-01-11-raspios-buster-armhf-lite.img1        8192  532479  524288  256M  c W95 FAT32 (LBA)
2021-01-11-raspios-buster-armhf-lite.img2      532480 3637247 3104768  1.5G 83 Linux
```

#### 使用 losetup 创建回环设备

由上图可知 `2021-01-11-raspios-buster-armhf-lite.img` 有 2 个分区，大小单位为 512 字节，转为字节计算后如下。
2021-01-11-raspios-buster-armhf-lite.img1：

- 开始=8192x512=4194304 大小=(532479-8192)x512=268434944

2021-01-11-raspios-buster-armhf-lite.img2：

- 开始=532480x512=272629760 大小=(3637247-532480)x512=1589640704

```bash
ubuntu@ubuntu-MRC-WX0:~$ sudo losetup -f -o 272629760 --sizelimit 1589640704 2021-01-11-raspios-buster-armhf-lite.img
ubuntu@ubuntu-MRC-WX0:~$ losetup -a
```

```
/dev/loop0: []: (/home/ubuntu/2021-01-11-raspios-buster-armhf-lite.img), offset 272629760, sizelimit 1589640704
```

#### 写入分区

```bash
sudo dd if=/dev/loop0 of=/dev/sdd3 bs=4M status=progress
sudo losetup --detach-all
```

写入后，可能由于镜像的原因都是分区变小了，不过这很好解决：

```bash
sudo e2fsck -f /dev/sdd3
sudo resize2fs /dev/sdd3
```

# 配置分区

首先我们给新的分区设置新的 lable，默认会是 rootfs： `e2label /dev/loop0p2 recoveryfs`

然后我们需要修改一些文件，让它们可以正确被引导。

1. 使用 `sudo blkid` 查看 `PARTUUID`，结果像下面这样：

   ```
   /dev/sdd1: LABEL="boot" UUID="6341-C9E5" TYPE="vfat" PARTUUID="ea7d04d6-01"
   /dev/sdd2: LABEL="rootfs" UUID="80571af6-21c9-48a0-9df5-cffb60cf79af" TYPE="ext4" PARTUUID="ea7d04d6-02"
   /dev/sdd3: LABEL="recoveryfs" UUID="80571af6-21c9-48a0-9df5-cffb60cf79af" TYPE="ext4" PARTUUID="ea7d04d6-03"
   ```

2. 将恢复分区挂载

   ```bash
   sudo mount /dev/sdd3 /mnt
   ```

3. 修改 `etc/fstab` 文件，修改对应的 PARTUUID

   ```bash
   proc            /proc           proc    defaults          0       0
   PARTUUID=ea7d04d6-01  /boot           vfat    defaults          0       2
   PARTUUID=ea7d04d6-03  /               ext4    defaults,noatime  0       1
   # a swapfile is not a swap partition, no line here
   #   use  dphys-swapfile swap[on|off]  for that
   ```

4. 修改 `boot` 中的 `cmdline.txt` 文件中的 PAERUUID 来指定启动分区

   ```
   console=serial0,115200 console=tty1 root=PARTUUID=ea7d04d6-02 rootfstype=ext4 elevator=deadline fsck.repair=yes quiet splash plymouth.ignore-serial-consoles
   ```

   将 `PARTUUID=` 后面的 8 个字符的代码改为我们查到到的值，并将下面的 `-02` 改为 `-03`，告诉 Raspbian 引导到第三个分区。

# 备份镜像

接下来就是要备份我们的主分区了，我们也是使用 `dd` 命令并通过 `gzip` 进行压缩。

**备份**

```bash
cd /mnt
sudo dd if=/dev/sdd2 bs=4M status=progress | gzip -6 > recovery.img.gz
```

**恢复**

```bash
gzip -dc recovery.img.gz | sudo dd of=/dev/sdd2 bs=4M status=progress
```

# 恢复脚本

我们将脚本创建在 `/boot` 中，这样在两个系统中都可以使用

**boot_to_recovery**

```bash
#!/bin/bash
if [ "$EUID" -ne 0 ]
  then echo "Please run as root"
  exit
fi

echo Rebooting to recovery partition in 5 seconds

if [ "$1" = "restore" ]; then
    echo Automatic restore selected
    touch /boot/restore
fi

sleep 5
sed -i 's/-02/-03/' /boot/cmdline.txt
touch /boot/ssh
reboot

exit 0
```

**boot_to_root**

```bash
#!/bin/bash
if [ "$EUID" -ne 0 ]
  then echo "Please run as root"
  exit
fi

echo Rebooting to root partition in 5 seconds
sleep 5
sed -i 's/-03/-02/' /boot/cmdline.txt
touch /boot/ssh
reboot
exit 0
```

**restore_root**

```bash
#!/bin/bash
if [ -f "/boot/restore" ]; then
    echo Restoring rootfs
    #dd if=/recovery.img of=/dev/mmcblk0p3 conv=fsync status=progress bs=4M
    gzip -dc recovery.img.gz | sudo dd of=/dev/sdd2 bs=4M status=progress
    unlink /boot/restore
    /boot/boot_to_root
fi

exit 0
```

### 自动恢复

如果你想在启动恢复分区时自动运行脚本，可以将它添加到恢复分区的 `/etc/rc.local` 文件，

在 `exit 0` 之前加上 `/boot/restore_root`。
