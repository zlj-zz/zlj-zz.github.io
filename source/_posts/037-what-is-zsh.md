---
title: Z-shell zsh 让终端不在只是终端
date: 2021-01-19 15:12
category: Linux
---

很久之前有说到 fish，一个开箱即用的优质 shell。这次来讲讲 zsh，让你深度定制自己的 shell。使你的终端独一无二。

![Screenshot](https://img-blog.csdnimg.cn/20210119000431880.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4NDEwNDk0,size_16,color_FFFFFF,t_70)

# 什么是 Zsh

Zsh 是一个强大的 shell，既可以作为交互式 shell，也可以作为脚本语言解释器。用户社区网站"[Oh My Zsh](https://github.com/ohmyzsh/ohmyzsh)"收集 Z shell 的第三方插件及主题。[8]截止于 2018 年，其 GitHub 源共有超过 1000 位贡献者、200 多款插件和超过 140 款主题。

相比于 bash，zsh 在很多方面做出了优化。例如：自带对数百条命令的补全支持；本身就可以通过文件扩展来匹配文件；自带拼写检查；可编程的命令行界面；自带一些常用的命令，如 `where`.于此同时，zsh 是兼容 bash 的，而 fish 并不是。

选择了 zsh，你会拥有比 bash 更好的使用体验，比 fish 更灵活的自定义空间。

# 安装 Zsh

## Linux

安装 zsh 是非常简单的，任何的 **Linux** 都可以使用自带的包管理器来安装 zsh， 例如 Ubuntu：

```bash
sudo apt install zsh
```

## Mac

Mac 从 2019 年开始就将默认的 shell 从 bash 替换成了 zsh，所以也许你根本不需要安装就已经拥有了 zsh。当然，如果你的 mac 是 2019 以前的，没有 zsh， 用 `brew` 安装它也十分容易。

```bash
brew install zsh
```

## Windows

在 windows 中安装 zsh 就显得麻烦多了。首先，你需要安装 [**Cygwim**](https://cygwin.com/install.html)，它可以帮你在在 windows 中构建 Unix 环境，在 windows 中感受 Liunx 的感觉。

运行安装程序时，请按照说明操作，确保安装程序安装在 `C:\cygwin64`。

![](https://img-blog.csdnimg.cn/20210119010300363.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4NDEwNDk0,size_16,color_FFFFFF,t_70#pic_center)

在包选择页面，搜索 wget 展开所有在 Web 下选择最新版本在 wget 的 bin 下，不需要选择源。然后继续

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210119010321466.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4NDEwNDk0,size_16,color_FFFFFF,t_70#pic_center)

我们将使用 Cygwin 安装一些软件包，这些软件包将有助于在 Windows 上使用 Linux 系统。我们将安装 `apt-cyg`，它将像 `apt-get` 一样工作，还将安装许多其他包，像 vim 一样。

```bash
wget rawgit.com/transcode-open/apt-cyg/master/apt-cyg
install apt-cyg /bin
apt-cyg install zsh git
```

## 让 Zsh 成为默认

在 Linux 和 Mac 中，你只需要打开 terminal， 输入下面的命令就可将 zsh 设置成默认的 shell。

```bash
sudo chsh -s $(which zsh)
```

# 配置你的 Zsh

一开进入 zsh，你可能觉得很一般。因为 zsh 的默认几乎和 bash 差不多。但 zsh 拥有许多强大的插件，这些插件可以在[Oh My Zsh](https://github.com/ohmyzsh/ohmyzsh)中找到。

这里呢，我推荐使用 [zimfw](https://github.com/zimfw/zimfw) 来安装插件。Zim 是一个 Zsh 配置框架，具有极快的速度和模块化扩展。Zim 非常容易定制，并且提供了一组丰富的模块和功能。

Zim 提供了两种安装方式。

- With curl:

  ```bash
    curl -fsSL https://raw.githubusercontent.com/zimfw/install/master/install.zsh | zsh
  ```

- With wget:

      ```bash
      wget -nv -O - https://raw.githubusercontent.com/zimfw/install/master/install.zsh | zsh
      ```

  然后，就可以在 `~/.zimrc` 中配置你想要的插件，这里我也推荐几个：

```
zmodule completion

zmodule zsh-users/zsh-completions

zmodule zsh-users/zsh-autosuggestions

# Fish-like history search (up arrow) for Zsh.
# zsh-users/zsh-history-substring-search must be sourced after zsh-users/zsh-syntax-highlighting
zmodule zsh-users/zsh-history-substring-search

# Enables and configures smart and extensive tab completion.
# completion must be sourced after zsh-users/zsh-completions
zmodule zdharma/fast-syntax-highlighting
```

配置好你想要的模块后，记得使用 `zimfw install` 来安装它们。

同样的，安装主题就和安装模块一样，放在配置文件中， install 就好了。

[https://github.com/zimfw/zimfw/wiki/Themes](https://github.com/zimfw/zimfw/wiki/Themes)

[https://github.com/ohmyzsh/ohmyzsh/wiki/Themes](https://github.com/ohmyzsh/ohmyzsh/wiki/Themes)

# 深度定制 Zsh

## 停止重复安装它

在完成插件和主题的安装后，想必你的 zsh 已经相当的好用了。

Ok，现在你基本配置好了你喜欢的 zsh，但如果换了电脑不就要再来一次这些安装过程，实在是太麻烦了。我们可以将配置上传到 github 上并写一个自动安装的脚本，这样下一次，我们只需要 clone 代码，运行脚本就重新配置好 zsh 了。

这里我给出我的脚本，作为参考。

```bash
export PLUG_DIR=$HOME/.zim
if [[ ! -d $PLUG_DIR ]]; then
    curl -fsSL https://raw.githubusercontent.com/zimfw/install/master/install.zsh | zsh
    rm ~/.zimrc
    ln -s ~/.config/zsh/zimrc ~/.zimrc
    echo "source ~/.config/zsh/zshrc">>~/.zshrc
    zimfw install
fi
```

## 像 vim 一样使用

如果你是一个 Vim 或者 Neovim 的使用者，想必一定十分喜爱 vim 的模式。那么，恭喜你，zsh 刚好支持 vim 模式和键位的绑定。

创建一个文件 `vi.zsh`，内容是：

```bash
bindkey -v
bindkey -M vicmd "i" vi-insert
bindkey -M vicmd "I" vi-insert-bol
bindkey -M vicmd "h" vi-backward-char
bindkey -M vicmd "l" vi-forward-char
bindkey -M vicmd "0" vi-beginning-of-line
bindkey -M vicmd "4" vi-end-of-line
bindkey -M vicmd "j" down-line-or-history
bindkey -M vicmd "k" up-line-or-history
bindkey -M vicmd "u" undo
#bindkey -M vicmd "-" vi-rev-repeat-search
bindkey -M vicmd "=" vi-repeat-search
bindkey -M vicmd "e" vi-forward-word-end
bindkey "^D" autosuggest-accept

function zle-line-init zle-keymap-select {
    RPS1="${${KEYMAP/vicmd/-- NOR --}/(main|viins)/-- INS --}"
    RPS2=$RPS1
    zle reset-prompt
}

function zle-keymap-select {
  if [[ ${KEYMAP} == vicmd ]] || [[ $1 = 'block' ]]; then
    echo -ne '\e[1 q'
  elif [[ ${KEYMAP} == main ]] || [[ ${KEYMAP} == viins ]] || [[ ${KEYMAP} = '' ]] || [[ $1 = 'beam' ]]; then
    echo -ne '\e[5 q'
  fi
}
zle -N zle-keymap-select

echo -ne '\e[5 q'

preexec() {
  echo -ne '\e[5 q'
}

_fix_cursor() {
  echo -ne '\e[5 q'
}
precmd_functions+=(_fix_cursor)

zle -N zle-line-init
zle -N zle-keymap-select

KEYTIMEOUT=1
```

然后在 `~/.zshrc` 中使用 `source` 命令载入它，你就可以像使用 vim 一样使用 zsh。

# 我的 Zsh

最后，贴上我的 zsh 配置，供有需要的人参考。
[https://github.com/zlj-zz/dotconfig/tree/master/zsh](https://github.com/zlj-zz/dotconfig/tree/master/zsh)
