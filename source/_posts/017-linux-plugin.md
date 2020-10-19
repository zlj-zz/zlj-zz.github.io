---
title: Interesting plugin in linux
date: 2020-05-03 13:33:21
category: Linux
tags:
---

If you are a programer, then you must know the Linux system. Even it's your main working environment. So can we have some **interesting**?

Next, I will introduce a few software that is interesting under **Linux**. Different Linux distributions may have different installation methods, the following installations use `Arch` as an example.

# FIGLET

FIGLET --display large characters made up of ordinary screen characters.

**Install**

```shell
sudo pacman -S figlet
```

**Display**

```shell
figlet hello world

watch -n1 "date '+%D%n%T'|figlet -k"   # terminal clok
```

![figlet](/images/linux-plugin/figlet.png)

# TOILET

TOIlet --display large colourful characters.

TOIlet prints text using large characters made of smaller characters. It is similar in many ways to FIGlet with additional features such as Unicode handling, colour fonts, filters and various export formats.

**Install**

```shell
sudo pacman -S toilet
```

**Display**

```shell
toilet -f mono12 "Takio"
```

![toilet](/images/linux-plugin/toilet.png)

# NEOFETCH

Neofetch --A fast, highly customizable system info script

Neofetch is a CLI system information tool written in BASH. Neofetch displays information about your system next to an image, your OS logo, or any ASCII file of your choice.

_NOTE: Every launch flag has a config option._

**Install**

```shell
sudo pacman -S neofetch
```

**Display**

```shell
neofetch
```

![neofetch](/images/linux-plugin/neofetch.png)

# SL

sl - cure your bad habit of mistyping

sl is a highly advanced animation program for curing your bad habit of mistyping.

**Install**

```shell
sudo pacman -S sl
```

**Display**

```shell
sl
```

![sl](/images/linux-plugin/sl.png)

# ASCIIQUARIUM

asciiquarium --Show a beautiful dynamic aquarium in terminal.

**Install**

```shell
sudo pacman -S asciiquarium
```

**Display**

```shell
asciiquarium
```

![asciiquarium](/images/linux-plugin/quarium.png)

# COWSAY

cowsay/cowthink --configurable speaking/thinking cow (and a bit more)

Cowsay generates an ASCII picture of a cow saying something provided by the user. If run with no arguments, it accepts standard input, word-wraps the message given at about 40 columns, and prints the cow saying the given message on standard output.

**Install**

```shell
sudo pacman -S cowsay
```

**Display**

```shell
cowsay "hello world"
```

![cowsay](/images/linux-plugin/cowsay.png)

# CMATRIXS

CMatrix --Shows a scrolling 'Matrix' like screen in Linux

**Install**

```shell
sudo pacman -S cmatrixs
```

**Display**

```shell
cmatrixs
```

![cmatrixs](/images/linux-plugin/cmatrixs.png)

# LOLCAT

lolcat --rainbow coloring effect for text console display

This manual page documents briefly the lolcat command.

lolcat is a program that concatenates files, or standard input, to standard output (like the generic cat), and adds rainbow coloring to it.

**Install**

```shell
sudo pacman -S lolcat
```

**Display**

```shell
neofetch | lolcat

cowsay -dragon "hello world" | lolcat
```

![lolcat-demo-1](/images/linux-plugin/lolcat-1.png)

![lolcat-demo-2](/images/linux-plugin/lolcat-2.png)

# ONEKO

oneko --The program oneko creates a cute cat chasing around your mouse cursor.

oneko changes your mouse cursor into mouse and creates a little cute cat and the cat start chasing around your mouse cursor. If the cat catchup the _mouse_, start sleeping.

**Install**

```shell
sudo pacman -S oneko
```

**Display**

```shell
oneko -speed 15
```

![oneko](/images/linux-plugin/oneko.png)

# FORTUNE

fortune --print a random, hopefully interesting, adage

**Install**

```shell
sudo pacman -S fortune
```

**Display**

```shell
fortune
```

![fortune](/images/linux-plugin/fortune.png)

# REV

rev --reverse lines characterwise

**Install**

```shell
sudo pacman -S rev-plugins
```

**Display**

```shell
rev
```

![rev](/images/linux-plugin/rev.png)

---

> To be continued ...
