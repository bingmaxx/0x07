# dotfiles 管理

> 2023-0725

`#Mac` `#Linux` `#Unix`

## 介绍

**dotfiles 点文件**：以 `.` 开头的文件，是类 `Unix` 系统的文件命名习惯，通常作为软件配置文件名。例如 `.bashrc` 是 `shell` 配置文件，`.vimrc` 是文本编辑器 `vim` 配置文件。

这种命名习惯据说（未考证）是在类 `Unix` 系统还没有图形界面时，使用 `ls` 命令默认不显示两个永恒的目录：当前目录 `.` 和上级目录 `..`，只有加上 `-a` 参数才显示，更进一步发现 `ls` 命令不显示所有以 `.` 开头的文件。而很多配置文件必须存在，但平时却并不怎么使用，于是索性以 `.` 开头，默认隐藏。

现在问题是，想把这些点文件管理起来，却发现它们散布在家目录顶级目录下，使用 `Git` 之类版本管理系统并不合适。解决方案不止一种，本人在用方案细节见 **阮一峰 dotfiles 仓库**[^1]，**【译】使用 GNU stow 管理你的点文件**[^2]。原理是为点文件创建 `symbolic links`（符号连接、软连接），而 [GNU stow](https://www.gnu.org/software/stow/) 是一个软连接管理器，使用它管理软连接更方便。

## 用法

现在以管理 `.vimrc` 文件为例简单介绍一下用法，首先查看文件目录结构：

```sh
# dotfiles 在家目录 ~ 下
├── dotfiles
│   ├── vim
│   │   ├── .vimrc
```

使用如下 `stow` 命令，为 `~/.vimrc` 创建指向 `dotfiles/vim/.vimrc` 的软连接：

```sh
# 在 dotfiles 目录下执行
$ stow vim

$ cd ~
$ ll -a | grep vimrc
lrwxr-xr-x 1 bingmax  staff  19  4  8 12:03 .vimrc -> dotfiles/vim/.vimrc
```

如上，可以将所有点文件集中在 `dotfiles` 目录中，再用版本管理系统管理起来。


## 解释

只解释 `stow` 命令部分用法，`stow` 目的是通过软连接让不同目录的软件包 `PACKAGE` 看起来像安装在同一个地方。主要有三个概念：

- **stow 目录** 包含所有 `stow` 软件包的目录，默认为当前目录。如上文 `dotfiles` 目录
- **stow 软件包** 特定软件包目录。如上文 `dotfiles/vim` 目录
- **stow target 目录** 软件包应该安装到的目录，默认为当前目录的父目录。如上文家目录 `~`

常用三个参数：

- `-d` 指定 **stow 目录**
- `-t` 指定 **stow target 目录**
- `-D` 移除已创建软连接

```sh
# 在 dotfiles 目录下执行
# stow 目录：当前目录 dotfiles
# stow 软件包：vim
# stow target 目录：~
# 软件包 vim 目录下所有文件，被 target 目录 ~ 下同名文件进行了软连接
$ stow vim -t ~

# 在 ~ 目录下执行
# 效果同上
$ stow vim -d dotfiles -t ~

# 在 dotfiles 目录下执行
# 移除已创建软连接
$ stow -D vim

# 在 ~ 目录下执行
# 效果同上
$ stow -D vim -d dotfiles
```

## 其它方案

上文说到解决方案不止一种，**使用 YADM 整理你的 dotfiles**[^3] 一文介绍了如何使用 [yadm](https://yadm.io/) 来管理 `dotfiles`。**General-purpose dotfiles utilities**[^4] 则罗列了非常非常多的点文件实用程序。

以及要想从根本上解决点文件散布在用户家目录顶级目录下这个问题，编写软件时采用已有 [XDG 目录规范](https://specifications.freedesktop.org/basedir-spec/basedir-spec-latest.html)，将配置文件和数据文件放在该规范指定位置即可。


[^1]: 阮一峰 dotfiles 仓库
https://github.com/ruanyf/dotfiles

[^2]:【译】使用 GNU stow 管理你的点文件
https://farseerfc.me/zhs/using-gnu-stow-to-manage-your-dotfiles.html

[^3]: 使用 YADM 整理你的 dotfiles
https://sspai.com/post/66894

[^4]: General-purpose dotfiles utilities
https://dotfiles.github.io/utilities
