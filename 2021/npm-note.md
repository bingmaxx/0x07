# npm 笔记
> 2021-0529

`Mac` `Windows` `Linux`

整理自 **npm** 官方文档（下文称文档） [docs.npmjs.com](https://docs.npmjs.com/cli/v7/commands/npm)，命令部分为 **Version 7.x**。

文档结构大致如下：
- npm 用户账户介绍
- 软件包、模块介绍与使用
- 外部服务集成 npm
- 团队与组织
- 企业级 npm
- npm 命令

本笔记分为三个部分：**npm 命令**、**npm 实践**、**相关生态**。省略了概念性的知识，重在信息汇总，最终需要回归文档与实践。

## npm 命令
以下为所有命令的汇总以及本人对命令的分类，遍历所有命令，可以知道命令能够做到哪些事。实际上不同命令使用频率不同，重要性也不同。感性的来说，拥有别名的命令需要更加重视。

文档中列出了 **62** 个命令：
`access` `adduser` `audit` `bin` `bugs` `cache` `ci` `completion` `config` `dedupe` `deprecate` `diff` `dist-tag` `docs` `doctor` `edit` `exec` `explain` `explore` `find-dupes` `fund` `help` `help-search` `hook` `init` `install` `install-ci-test` `install-test` `link` `logout` `ls` `org` `outdated` `owner` `pack` `ping` `prefix` `profile` `prune` `publish` `rebuild` `repo` `restart` `root` `run-script` `search` `set-script` `shrinkwrap` `star` `stars` `start` `stop` `team` `test` `token` `uninstall` `unpublish` `unstar` `update` `version` `view` `whoami`

其中 `npx = npm exec`，因此未计数。使用 `npm help` 可以在命令行中列出所有命令，会发现比文档中的略多，比如 `ll` `get` `set`，其中 ll 是 ls 命令的别名，get、set 等价于 config 的 get、set 子命令。

将所有的命令进行分类：

### 账号相关 `7`
当需要对 npm 进行身份验证时，有两种选择：用户名/密码、token。只有完成了身份验证，才能做发包（publish）、删除包（unpublish）等操作。
除了个人身份验证，team、org 命令还可以管理组织与团队。
profile 命令可以修改账户的个人信息。
- `npm adduser` = `npm [login | add-user]`
- `npm logout`
- `npm whoami`
- `npm token`
- `npm team`
- `npm org`
- `npm profile`

### 管理自己的包 `6`
完成身份验证后可以发包（publish）、删除包（unpublish）。其它命令还包括：
access：修改已发布包的访问级别。
deprecate：弃用包的某个版本。
dist-tag：添加/删除/列举包的标签。在安装包时，标签等同于版本号。
owner：管理包的所有者。
- `npm publish`
- `npm unpublish`
- `npm access`
- `npm deprecate`
- `npm dist-tag` = `npm dist-tags`
- `npm owner` = `npm author`

### 查看 npm 信息 `6`
以下命令可以检查 npm 版本、帮助命令、相关目录位置。
- `npm version` 类似于 `npm [-v | --version]`
- `npm help` = `npm -h`
- `npm help-search`
- `npm bin`
- `npm root`
- `npm prefix`

### npm 环境 `5`
一些对 npm 环境进行操作的命令：
ping：ping 一下 npm 的源。
doctor：执行包括 ping 在内的一些列命令，用于检查 npm 环境。
cache：管理包缓存。
config：管理 npm 的配置。
completion: npm 命令自动补全。
- `npm ping`
- `npm doctor`
- `npm cache`
- `npm config` = `npm c`
- `npm completion`

### 脚本命令 `7`
推荐阅读阮一峰老师的 **npm scripts 使用指南**[^1]
- `npm start`
- `npm restart`
- `npm stop`
- `npm test` = `npm [t | tst]`
- `npm rebuild` = `npm rb`
- `npm run-script` = `npm [run | rum | urn]`
- `npm set-script`

### 项目中使用包 `17`
init 命令可以交互式的创建 package.json 文件，一般在全新的项目中使用。
install、uninstall、update 是常用的安装/卸载/更新包命令。
install-test、ci、install-ci-test 则是稍微特别一点的安装命令。
link 命令用于在 node_modules 目录下建立一个符号连接，指向指定的包或者包所在目录。
prune 命令删除无关（存在于 node_modules 目录中但是没有包依赖它）的包。
outdated 命令检查过时的包。
dedupe 命令**减少**包树中的重复。
find-dupes 命令**查找**包树中的重复。
explain 命令打印包在项目中的依赖链。
explore 命令进入指定包中执行一些命令。
audit 命令进行安全审计，使用 fix 参数可以直接安装更安全的依赖版本。
exec 命令允许从已安装的包中运行命令。等价于 npm@5.2 中新增的 npx 命令。推荐阅读阮一峰老师的 **npx 使用教程**[^2]
shrinkwrap 命令锁定项目中包的依赖版本。
- `npm init`
- `npm install` = `npm [i | add]`
- `npm uninstall` = `npm [remove | rm | r | un | unlink]`
- `npm update` = `npm [up | upgrade]`
- `npm install-test` = `npm it`
- `npm ci`
- `npm install-ci-test` = `npm cit`
- `npm link` = `npm ln`
- `npm prune`
- `npm outdated`
- `npm dedupe` = `npm ddp`
- `npm find-dupes`
- `npm explain` = `npm why`
- `npm explore`
- `npm audit`
- `npm exec` = `npm x` = `npx`
- `npm shrinkwrap`

### 获取/编辑包信息 `10`
search 命令可以检索所有已发布包，作用与在 npm 官网检索相同。
ls 命令列出项目中或全局安装的包。
view 命令查看包注册表信息。
diff 命令查看不同版本包的差异。
docs 命令在浏览器中打开包的文档。
bugs 命令在浏览器中打开反馈 bug 的地址。
repo 命令在浏览器打开包存储库界面。
fund 命令在浏览器打开给项目提供资金的界面。
pack 命令给包创建一个 tarball。
edit 命令使用默认编辑器打开包目录供用户编辑包信息。
- `npm search` = `npm [s | se | find]`
- `npm ls` = `npm [list | la | ll]`
- `npm view` = `npm [info | show | v]`
- `npm diff`
- `npm docs` = `npm home`
- `npm bugs` = `npm issues`
- `npm repo`
- `npm fund`
- `npm pack`
- `npm edit`

### 用于自动化的钩子 `1`
首先什么是钩子？钩子是 npm 注册表事件的通知。hook 命令可以做到订阅指定包的注册表事件，当包发生变化（publish、dist-tag……）时，会向 hook 命令配置的 URL 发送 HTTP POST。
- `npm hook`

### 操作喜欢的包 `3`
- `npm star`
- `npm unstar`
- `npm stars`

## npm 实践

### npm 配置
npm 获取配置的优先级依次为：命令行参数、环境变量、`.npmrc` 文件、默认配置。
一般使用 config 命令管理配置：

```sh
npm config list -l              # 获取所有配置
npm config get <key>            # 获取指定配置
npm config delete <key>         # 删除指定配置

npm config set <key>=<value>    # 设置配置值，配置会出现在 .npmrc 文件中
# npm config set <key> <value>  # 同上
# npm c set <key> <value>       # 同上
# npm set <key> <value>         # 同上
```

一次性修改配置可以使用命令行参数：
```sh
npm <command> --<key>           # key = true
npm <command> --<key>=<value>   # key = value
```

由于 npm 官方源访问速度问题，开发中经常会配置源地址，甚至出现了专门的包 **nrm** 用于管理 npm 源。
源配置对应 `registry` 字段。常见源地址及配置如下：
```shell
# npm -----  https://registry.npmjs.org/
# yarn ----- https://registry.yarnpkg.com
# cnpm ----  http://r.cnpmjs.org/
# taobao --  https://registry.npm.taobao.org/
# nj ------  https://registry.nodejitsu.com/
# skimdb --  https://skimdb.npmjs.com/registry

npm config get registry             # 查看当前源地址
npm config set registry <url>       # 设置源地址
```

### 安装包
`install` 命令用于安装包。几乎是最重要的命令，其概览如下：
```sh
npm install (with no args, in package dir)
npm install [<@scope>/]<name>
npm install [<@scope>/]<name>@<tag>
npm install [<@scope>/]<name>@<version>
npm install [<@scope>/]<name>@<version range>
npm install <alias>@npm:<name>
npm install <git-host>:<git-user>/<repo-name>
npm install <git repo url>
npm install <tarball file>
npm install <tarball url>
npm install <folder>
```

首先解释一下命令行格式中符号含义（未找到官方定义）：
- `[]` 可选项
- `<>` 可变选项（多选一，且必须选一）
- `x|y|z` 多选一
- `-abc` 多选

`npm install`：当命令后不接包相关信息时，package.json 文件中 `dependencies` 和（或） `devDependencies` 字段定义的包将安装到本地 node_modules 目录。具体规则受配置字段 `production` 控制。
- `production = true` 仅安装 dependencies 中的包
- `production = false` dependencies 和 devDependencies 中的包都安装

命令具体使用如下：
```sh
# 命令行中修改配置并安装
npm install --production          # production = true
npm install --production=false    # production = false

# 修改配置文件(长期有效)并安装
npm set production=true
npm set production=false
npm install
```

`npm install <pkg>`：命令后接包名，此时从源地址拉取安装包的最新版。包名后接 `tag` 或版本号（可带范围）可以安装指定版本的包。对照上面的指令概览，包也可以为 git 仓库地址、包 tarball 文件、包 tarball 文件的地址、包目录等。

此外，命令后可添加参数：

- `--save-prod` = `-P` 默认情况。包出现在 dependencies
- `--save-dev` = `-D` 包出现在 devDependencies
- `--save-optional` = `-O` 包出现在 optionalDependencies
- `--save-bundle` = `-B` 包出现在 bundleDependencies
- `--no-save` 包不出现在 package.json 文件中
- `--save-exact` = `-E` 安装包的精确版本

`--save-exact` 参数控制安装包的精确版本（no `~` or `^`），其它参数控制包出现在 package.json 文件中的位置。重点讲一下 dependencies 与 devDependencies 的区别。

语义上 devDependencies 是指在开发过程中使用的包，项目正式运行时不需要。效果上项目 `project_a` 安装一个包 `pkg_a`，pkg_a devDependencies 中的包都不会在 project_a 中安装。pkg_a dependencies 中的包则会在 project_a 中安装并遵循依赖加载算法。

文档中关于该算法的举例：

存在重复包。
```sh
# 项目中依赖包的拓扑结构
project
├── A
│   ├── B
│   ├── C
├── B
│   ├── C
├── C
│   ├── D

# 实际 node_modules 目录结构
project
├── node_modules
│   ├── A
│   │   ├── node_modules
│   │   │   ├── B
│   │   │   ├── C
│   │   │   ├── D
```

存在同名但不同版本的包
```sh
# 项目中依赖包的拓扑结构
project
├── A
│   ├── B
│   ├── C
├── B
│   ├── C
│   ├── D@1
├── C
│   ├── D@2

# 实际 node_modules 目录结构
project
├── node_modules
│   ├── A
│   │   ├── node_modules
│   │   │   ├── B
│   │   │   ├── C
│   │   │   │   ├── node_modules
│   │   │   │   │   ├── D@2
│   │   │   ├── D@1
```

### 开发包的小技巧 `link`
`link` 命令可以通过符号链接（symlink）指向指定包。当指向自己开发的包时，可以随时重构开发包的内容，提高开发效率。
`install` 命令后接包目录时，可以实现同样效果。

假设开发包为 `pkg_dev`，目录为 `{path}/pkg_dev`。
在项目 `pkg_pro` 中通过 link 命令使用开发包，最终项目结构如下：

```
pkg_pro
├── node_modules
│   ├── pkg_dev -> {path}/pkg_dev
```

命令具体使用如下：
```sh
# 用法 ①
# 在全局 node_modules 目录创建指向当前包的符号链接
npm link (in package dir)
# 当前项目 node_modules 目录创建指向包的符号链接
# 如果包不存在则先全局安装
npm link <pkg>

# 用法 ②
# 当前项目 node_modules 目录创建指向指定目录的符号链接
npm link <pkg_directory>

# 等效用法 ③
# 当前项目 node_modules 目录创建指向指定目录的符号链接
# 同时 package.json 文件 devDependencies 中出现包名、包所在目录
npm install <pkg_directory>
```

### 发布包
当编写了一个含有 package.json 的包后，只要完成用户身份验证即可发包，记得包不得与已有包重名。一个完整的流程如下：
```sh
# 交互式：输入用户名、密码、电子邮件地址（前提是已在网站注册 npm 账号）
npm login                       # login 是 adduser 的别名
# npm whoami                    # 登陆成功会显示用户名
# npm profile get               # 从 CLI 查看用户信息

# npm version <update_type>     # 修改 package.json 中的版本号
npm publish                     # 发布包
# npm publish --tag <tag>       # 发布包并为包添加 tag

npm unpublish <pkg> --force     # 取消发布整个包
# npm unpublist <pkg>@<version> # 删除包的指定版本
```

## 相关生态
**[node.js](https://nodejs.org/en)** is a JavaScript runtime built on Chrome's V8 JavaScript engine.（基于 Chrome 的 V8 引擎构建的 js 运行时。）
**[npm](https://www.npmjs.com/)** (node.js package manager) nodejs 的包管理器。
**[cnpm](https://npm.taobao.org)** 是淘宝 npm 镜像，它的仓库是 npm 仓库的一个拷贝；支持 npm 除了 publish 之外的所有命令。
**[nrm](https://www.npmjs.com/package/nrm)** 是一个 npm 源管理器，允许快速地在 npm 源间切换。
**[nvm](https://github.com/creationix/nvm)** (node.js version management) Mac 的 nodejs 版本管理工具。
**[nvm-windows](https://github.com/coreybutler/nvm-windows/releases)** Windows 的 nodejs 版本管理工具。


[^1]: npm scripts 使用指南
https://www.ruanyifeng.com/blog/2016/10/npm_scripts.html
[^2]: npx 使用教程
https://www.ruanyifeng.com/blog/2019/02/npx.html
