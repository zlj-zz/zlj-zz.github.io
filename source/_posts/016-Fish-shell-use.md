---
title: fish shell, young people's shell
date: 2020-05-02 10:16:08
category: Linux
tags:
- fish

---


# what's fish?

fish is a smart and user-friendly command line shell for macOS, Linux, and the rest of the family. fish includes features like syntax highlighting, autosuggest-as-you-type, and fancy tab completions that just work, with no configuration required.

![fish shell](/fish-shell-use/fish-shell.png)

# how to install?

Installing `fish` is really easy. If you are **Linux** or **MaxOS**, your system has its own package manager, you can use it. But different system may be different package managers.

Like me, type in the command line:
```shell
sudo pacman -S fish
```

On Windows 10, fish can be installed under the WSL Windows Subsystem for Linux with `sudo apt install fish` or from source with the instructions below. Fish can also be installed on all versions of Windows using [Cygwin](https://cygwin.com/)(from the Shells category).

If there is a problem during the installation, you can check the [github of fish](https://github.com/fish-shell/fish-shell). After you install it, enter `fish` on the command line to start using it, and it is already easy to use.

**Make it the default shell** 

When you use it for a while, and you like it very much. You want to set it as the default shell, how to do it?

It's very easy, just do that:
```shell
chsh -s /usr/local/bin/fish
```

`chsh` will prompt you for your password and change your default shell. (Substitute `/usr/local/bin/fish` with whatever path fish was installed to, if it differs.) Log out, then log in again for the changes to take effect.

# Customize your fish

Now you fall in love with `fish`, you want to make it a little different.

If you have a graphical interface, enter `fish_config` on the command line to setting you `fish`. Like this:

![fish_config](/fish-shell-use/fish_config.png) 

You think this is not enough, you want to make your `fish` more special. Then you can use `oh-my-fish`.

`oh-my-fish` provides core infrastructure to allow you to install packages which extend or modify the look of your shell. It's fast, extensible and easy to use. You can enter its github from [here](https://github.com/oh-my-fish/oh-my-fish).

I install `oh-my-fish` use git, like this:
```shell
$ git clone https://github.com/oh-my-fish/oh-my-fish
$ cd oh-my-fish
$ bin/install --offline
```

Use `omf` to use it. `oh-my-fish` offers interesting plugins and a lot of themes.

**Theme**

You can use `omf theme` to view installed and all themes. `omf install <theme name>` to install theme. `omf theme <theme name>` to use the theme of you choice.

Look at the preview at the beginning of this article, that is the theme I used. You can preview more themes [here](https://github.com/oh-my-fish/oh-my-fish/blob/master/docs/Themes.md).

**Plugin**

Here we recommend a plugin to check the weather in the terminal. 

Install through `omf install wttr`, use through `wttr`.
![wttr](/fish-shell-use/wttr.png) 
