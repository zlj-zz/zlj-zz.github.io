---
title: How to use Pacman ?
date: 2020-06-28
category: Linux
---

### S 命令

```bash
# 安装
sudo pacman -S git

# 更新
sudo pacman -Syy
sudo pacman -Syyu

# 搜索
sudo pacman -Ss git

# 清除旧安装包
sudo pacman -Sc
```

### R 命令

```bash
# 卸载
sudo pacma -R git

# 连同依赖的卸载
sudo pacman -Rs git

# 连同系统配置文件卸载
sudo pacman -Rns git
```

### Q 命令

```bash
# 列出已安装软件
pacman -Q

# 统计个数
pacman -Q | wc -l

# 列出你安装的软件
pacman -Qe

# 只显示名字不带版本号等
pacman -Qq

# 显示不需要的依赖
pacman -Qdt

# 搜索本地仓库
pacman -Qs

```
