# nvm 笔记

> 2023-0709

`#Mac` `#Linux` `#WSL` `#Unix` `#POSIX` `#nvm`
 
**[nvm](https://github.com/nvm-sh/nvm)** **node version manager** 是 `nodejs` 版本管理工具。用以解决开发中不同项目需要的 nodejs 版本不同的问题。以下内容主要来源于 `nvm` 项目的 `README` 文档。

## 安装
文档中提到的安装方式包括：
- 手动、`cURL`、`Wget` 运行安装脚本
- 手动 `clone` 源码安装，需要修改 `shell` 配置文件 `~/.bashrc` 或 `~/.profile` 或 `~/.zshrc`
- 如果使用 `zsh` 可以通过安装 [zsh-nvm](https://github.com/lukechilds/zsh-nvm) 插件来使用 nvm
- `Homebrew` 安装（不建议）
- `Docker` 镜像（仅推荐开发用途）

本人环境为 Mac、zsh，采用手动 `clone` 源码安装：
```sh
# 源码将放在 $HOME/.nvm 目录，然后加载 nvm
export NVM_DIR="$HOME/.nvm" && (
  git clone https://github.com/nvm-sh/nvm.git "$NVM_DIR"
  cd "$NVM_DIR"
  git checkout `git describe --abbrev=0 --tags --match "v[0-9]*" $(git rev-list --tags --max-count=1)`
) && \. "$NVM_DIR/nvm.sh"
```

下一步在 `~/.zshrc` 文件中添加以下行。点文件管理可参考 **dotfiles 管理**[^1]
```sh
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
```

验证是否已安装 nvm：

```sh
$ command -v nvm
nvm
```

### 更新
适用于本人的安装方式。
```sh
(
  cd "$NVM_DIR"
  git fetch --tags origin
  git checkout `git describe --abbrev=0 --tags --match "v[0-9]*" $(git rev-list --tags --max-count=1)`
) && \. "$NVM_DIR/nvm.sh"
```

### 卸载
适用于本人的安装方式。
```sh
rm -rf "$NVM_DIR"
```
然后删除 `~/.zshrc` 文件中的添加行。


## 使用

首先遍历 nvm 的所有命令，`$ nvm` <kbd>Tab</kbd> 自动补全会列出所有可用命令，结合 `nvm --help` 输出的帮助文档，整理后共 **21** 个命令：

- `--help` = `-h` = `help` 显示帮助文档
- `--version` = `-v` 显示安装的 nvm 版本
- `version` 将描述（别名）解析为本地 node 版本号
- `version-remote` 将描述（别名）解析为远程 node 版本号
- `install` = `i` 下载并安装指定版本，如果有 .nvmrc 文件，可省略版本号
- `uninstall` 卸载指定版本
- `use` 修改 PATH 变量以使用指定版本，如果有 .nvmrc 文件，可省略版本号
- `exec` 使用指定版本运行输入的命令，如果有 .nvmrc 文件，可省略版本号
- `run` 使用指定版本、参数运行 node 命令，如果有 .nvmrc 文件，可省略版本号
- `current` 显示当前激活的 node 版本
- `list` = `ls` 列出已安装的版本
- `list-remote` = `ls-remote` 列出可安装的远程版本
- `deactivate` 撤销 nvm 对当前 shell 的影响（恢复 PATH 变量）
- `alias` 为版本设置别名
- `unalias` 删除别名
- `install-latest-npm` 将当前版本上的 npm 升级到最新
- `reinstall-packages` 为当前版本重新安装全局 npm 包
- `unload` 从 shell 中卸载 nvm
- `which` 显示已安装版本的路径，如果有 .nvmrc 文件，可省略版本号
- `cache` 显示/清除 nvm 缓存
- `set-colors` 设置五种文本颜色


### 常用命令
```sh
nvm ls-remote   # 查看 node 所有发行版
nvm ls          # 查看 node 已安装版本

nvm install <version>                           # 安装 node 指定版本
nvm alias <alias_name> <version | alias_name>   # 为 node 版本设置别名
nvm use <version | alias_name>                  # 使用 node 指定版本
```

命令中的版本，可以使用版本号如 `v14.20.0` 来表示，也可以使用别名来表示：
```sh
# 特殊默认别名
default         # 默认版, 每个新 shell 中使用此版本
system          # 系统安装的版本
node            # 最新版
iojs            # 最新版 io.js
stable          # 已弃用，目前是 node 的别名
unstable        # 自 1.0 后，所有版本都是稳定的

# LTS（Long Term Support）每版都有一个代号，从元素周期表挑选一个合适的元素名，按照字母表排序
lts/argon       # 氩 v4.9.1
lts/boron       # 硼 v6.17.1
lts/carbon      # 碳 v8.17.0
lts/dubnium     # 𬭊 v10.24.1
lts/erbium      # 铒 v12.22.12
lts/fermium     # 镄 v14.21.3
lts/gallium     # 镓 v16.20.1
lts/hydrogen    # 氢 v18.16.1
```

### .nvmrc 文件
在项目目录下创建 `.nvmrc` 文件并写入版本号（或别名），`nvm use` 命令无参数时将使用该文件中指定的版本（原理是从当前目录向上遍历以查找 .nvmrc 文件）。同理 `nvm install`, `nvm exec`, `nvm run`, `nvm which` 命令无参数时也会使用该文件中指定的版本。

```sh
$ touch .nvmrc
$ echo v16.20.0 > .nvmrc

$ nvm use
Found '/Users/bingmax/.nvmrc' with version <v16.20.0>
Now using node v16.20.0 (npm v8.19.4)
```
与 shell 做更进一步的集成，还可以做到更改目录时自动调用 `nvm use`。具体用法此处略过。


### 全局 npm 包管理

首先查看一下本人当前 node 版本（`v14.20.0`）下全局安装的 npm 包有哪些：
```sh
$ npm list -g --depth 0
/Users/bingmax/.nvm/versions/node/v14.20.0/lib
├── corepack@0.10.0
├── npm@6.14.17
└── yarn@1.22.19
```

假设现在想安装 `v16.20.0` 版本，并希望迁移 `v14.20.0` 版本已安装的 npm 包，可以执行以下命令：
```sh
$ nvm install v16.20.0 --reinstall-packages-from=v14.20.0

$ npm list -g --depth=0
/Users/bingmax/.nvm/versions/node/v16.20.0/lib
├── corepack@0.10.0
├── npm@9.6.4
└── yarn@1.22.19
```

如果每次安装新版本时都有要默认安装的 npm 包，将包名称（每行一个）添加到 `$NVM_DIR/default-packages` 文件中即可：
```sh
$ cd $NVM_DIR
$ touch default-packages

# default-packages 文件写入以下内容
rimraf
object-inspect@1.0.2
stevemao/left-pad
```


### node 二进制文件镜像

```sh
# node 二进制文件镜像
$ export NVM_NODEJS_ORG_MIRROR=https://nodejs.org/dist
$ nvm install node

# iojs 二进制文件镜像
export NVM_IOJS_ORG_MIRROR=https://iojs.org/dist
nvm install iojs-v1.0.3
```

### 自定义颜色
可以设置五种颜色，用于显示版本和别名信息，还可以禁用颜色输出。具体用法此处略过。


## 原理

源码看了但是没看明白，根据实际操作的现象再结合文档，对原理解释如下。

首先看一下不同版本 node 的存放路径：
```sh
$ where node
/Users/bingmax/.nvm/versions/node/v14.20.0/bin/node

# 所有安装的 node 会按版本号放在如下目录
$ ll /Users/bingmax/.nvm/versions/node
total 0
drwxr-xr-x@  9 bingmax  staff  288  4 10 14:00 v14.20.0
drwxr-xr-x@  9 bingmax  staff  288  6  3 10:21 v16.20.0
drwxr-xr-x   9 bingmax  staff  288  5 30 07:18 v18.16.0
drwxr-xr-x   9 bingmax  staff  288  6 21 17:05 v20.3.1
```

本人安装了四个版本的 node，当前使用的版本为 `v14.20.0`。nvm 将 `/Users/bingmax/.nvm/versions/node/v14.20.0/bin` 路径放在了 `$PATH` 的最前面，这样就可以在全局找到当前版本的 node 命令。该路径会在使用 `nvm use` 命令切换 node 版本时被修改。
```sh
$ echo $PATH
/Users/bingmax/.nvm/versions/node/v14.20.0/bin:***
$ where node
/Users/bingmax/.nvm/versions/node/v14.20.0/bin/node

$ nvm use v16.20.0
Now using node v16.20.0 (npm v8.19.4)

$ echo $PATH
/Users/bingmax/.nvm/versions/node/v16.20.0/bin:***
$ where node
/Users/bingmax/.nvm/versions/node/v16.20.0/bin/node
```

**一个知识点**：`$PATH` 变量作用域为当前 shell 及其子进程，因此我们可以同时在不同的 shell 中使用不同版本的 node。

## 其它

### node 与 npm 版本对应
见 [nodejs 以往的版本](https://nodejs.org/zh-cn/download/releases)，部分摘抄如下：

Version	| LTS	| Date	| V8 | npm
-|:-:|:-:|-|-
Node.js 20.3.1   | 	        | 2023-06-20 | 11.3.244.8  |	9.6.7
Node.js 19.9.0   | 	        | 2023-04-10 | 10.8.168.25 |	9.6.3
Node.js 18.16.1  | Hydrogen	| 2023-06-20 | 10.2.154.26 |	9.5.1
Node.js 17.9.1   | 	        | 2022-06-01 | 9.6.180.15  |	8.11.0
Node.js 16.20.1  | Gallium	| 2023-06-20 | 9.4.146.26  |	8.19.4
Node.js 15.14.0  | 	        | 2021-04-06 | 8.6.395.17  |	7.7.6
Node.js 14.21.3  | Fermium	| 2023-02-16 | 8.4.371.23  |	6.14.18
Node.js 13.14.0  | 	        | 2020-04-29 | 7.9.317.25  |	6.14.4
Node.js 12.22.12 | Erbium	  | 2022-04-05 | 7.8.279.23  |	6.14.16
Node.js 11.15.0  | 	        | 2019-04-30 | 7.0.276.38  |	6.7.0
Node.js 10.24.1  | Dubnium	| 2021-04-06 | 6.8.275.32  |	6.14.12
Node.js 9.11.2   | 	        | 2018-06-12 | 6.2.414.46  |	5.6.0
Node.js 8.17.0   | Carbon	  | 2019-12-17 | 6.2.414.78  |	6.13.4
Node.js 7.10.1   | 	        | 2017-07-11 | 5.5.372.43  |	4.2.0
Node.js 6.17.1   | Boron	  | 2019-04-03 | 5.1.281.111 |	3.10.10
Node.js 5.12.0   | 	        | 2016-06-23 | 4.6.85.32   |	3.8.6
Node.js 4.9.1    | Argon	  | 2018-03-29 | 4.5.103.53  |	2.15.11
Node.js 0.12.18  | 	        | 2017-02-22 | 3.28.71.20  |	2.15.11

### 其它 nodejs 版本管理工具
**[nvm-windows](https://github.com/coreybutler/nvm-windows)** 适用于 `Windows`   
**[nodist](https://github.com/nullivex/nodist)** 适用于 `Windows`   
**[nvs](https://github.com/jasongin/nvs)** 适用于 `Windows`、`Mac`、`Linux`  
**[n](https://github.com/tj/n)** 适用于 `Mac`、`Linux`   


[^1]: dotfiles 管理
[dotfiles](./dotfiles.md)