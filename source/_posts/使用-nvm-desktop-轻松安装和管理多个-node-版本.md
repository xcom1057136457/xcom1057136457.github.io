---
title: 使用 nvm-desktop 轻松安装和管理多个 node 版本
date: 2025-01-02 09:58:07
---

## 前言

作为一枚前端开发工程师，我们一般都是通过在 [官网](https://nodejs.org) 下载二进制安装包来安装 `node`，但是实际开发难免会因为一些兼容性的问题需要安装切换不同版本的 `node`，如果还是通过这种方式，那么就必须先卸载现有版本再安装新的版本，无疑十分不便。

虽然社区已经有很成熟的工具（[nvm](https://github.com/nvm-sh/nvm)）来解决这个问题，但是 `nvm` 是基于 `shell` 的交互式命令的，用起来可能还是不是那么直观便捷：比如在 `macOS` 平台需要安装支持 `arm64` 架构的版本的 `node`，`nvm` 就没办法通过命令（`nvm ls -remote`）来查看；而在 `Windows` 平台则需要通过 [`nvm-windows`](https://github.com/coreybutler/nvm-windows) 来单独安装以获得支持。

今天就来介绍一个通过可视化界面操作来实现安装和管理多个 `node` 版本的工具：[`nvm-desktop`](https://github.com/1111mp/nvm-desktop)，全称：`Node Version Manager Desktop`，支持双平台，代码完全开源，可放心使用。

现在已经支持为不同项目单独设置和切换不同版本的 `Node`，开箱即用，不需要任何其他操作。

## 使用截图

`macOS` 下的演示视频：

![](https://raw.githubusercontent.com/xcom1057136457/DrawingBed/main/3df28a8154d044a1ae1e524997482fd3%7Etplv-k3u1fbpfcp-jj-mark%3A3024%3A0%3A0%3A0%3Aq75.awebp)

`Windows` 截图（使用效果是一样的，就没有单独再录制一份了）：

![](https://raw.githubusercontent.com/xcom1057136457/DrawingBed/main/20250102100338.png)

## 下载和安装

### 下载

下载地址：[`nvmd-desktop Download Page (GitHub release)`](https://github.com/1111mp/nvm-desktop/releases)

![](https://raw.githubusercontent.com/xcom1057136457/DrawingBed/main/20250102100455.png)

![](https://raw.githubusercontent.com/xcom1057136457/DrawingBed/main/20250102100506.png)

根据不同的平台，下载最新的版本。

### 安装

下载好对应平台的二进制安装程序之后，点击安装即可。

#### `macOS`

因为该程序目前并没有通过 **Apple开发者账号** 进行代码证书签名，所以在macOS上运行会出现安全提示：

> `"File/App is damaged and cannot be opened. You should move it to Trash."`

> `"File/App is damaged and cannot be opened. You should move it to Trash." is a Mac error that can occur to various macOS versions, such as macOS Ventura/Monterey/Big Sur/Catalina, especially on M1 Macs. It usually happens on apps or files downloaded from the web, but it can also arise when opening apps downloaded from App Store.`

这个是 `macOS` 的一种安全策略，特别是在最新的系统版本（`macOS Ventura` / `Monterey` / `Big Sur` / `Catalina` ）中，Apple 已经默认不允许用户通过系统设置来运行不受信任的应用程序。

可以查看 `nvm-desktop` 的 [文档](https://github.com/1111mp/nvm-desktop#macos-issues) 或者这篇文章（[`Fix 'File/App is damaged and cannot be opened' on Mac`](https://iboysoft.com/news/app-is-damaged-and-cannot-be-opened.html#:~:text=If%20you%27re%20certain%20the%20file%20or%20app%20is,and%20selecting%20Open%20twice.%204%20Restart%20your%20Mac.)）解决以允许该程序运行。

因为该项目代码完全开源，所以请放心运行。或者也可以根据 [文档](https://github.com/1111mp/nvm-desktop#build-and-package) 的教程，把代码 `clone` 到本地编译安装。

#### `Windows`

该程序在 `Windows` 平台默认申请 **管理员权限** 运行，因为需要将安装的 `node` 的文件路径设置到系统的环境变量中。

相关代码：

[编译打包的配置项：`requestedExecutionLevel`](https://github.com/1111mp/nvm-desktop/blob/main/package.json#L202)

[设置环境变量的代码：`setx -m NVMD nodePath`](https://github.com/1111mp/nvm-desktop/blob/main/src/main/utils/nvmdShell.ts#L64)

### 注意⚠️

`Release v2.0.0` 底层实现上做了大变动，现在不依赖操作系统的特定功能和`shell`，也不需要通过 `setx -m` 命令设置系统环境变量，更多详细信息请查看这篇文章：[Node 版本管理工具：nvm-desktop 进化啦！不依赖操作系统的功能和 shell，完美支持为项目切换不同的 Node 版本](https://juejin.cn/post/7276716177119772733)

## 运行

以 `macOS` 平台为例：

![](https://raw.githubusercontent.com/xcom1057136457/DrawingBed/main/20250102101327.png)

![](https://raw.githubusercontent.com/xcom1057136457/DrawingBed/main/20250102101338.png)

![](https://raw.githubusercontent.com/xcom1057136457/DrawingBed/main/20250102101350.png)

根据向导，需将如下命令添加到系统的 `~/.bashrc`、 `~/.profile` 或者 `~/.zshrc` 文件中：

```` shell
export NVMD_DIR="$HOME/.nvmd" 
[ -s "$NVMD_DIR/nvmd.sh" ] && . "$NVMD_DIR/nvmd.sh" # This loads nvmd
````

最新的 [`Release v2.0.0`](https://github.com/1111mp/nvm-desktop/releases/tag/v2.0.0) 版本则是：

```` shell
export NVMD_DIR="$HOME/.nvmd"
export PATH="$NVMD_DIR/bin:$PATH"
````

我的电脑用的是系统默认的 `zsh`, 所以复制这个命令添加到 `~/.zshrc` 文件中即可。如果你的电脑使用的是 `bash`，则复制粘贴到 `~/.bashrc` 文件中去即可。

`Windows` 下则不需要额外的操作，安装好运行之后直接搜索指定的 `node` 版本点击下载安装即可。

进入到主页面：

![](https://raw.githubusercontent.com/xcom1057136457/DrawingBed/main/20250102101612.png)

`node` 从 `v16.0.0` 开始支持 `macOS` 的 `arm64` 架构：

![](https://raw.githubusercontent.com/xcom1057136457/DrawingBed/main/20250102101650.png)

为不同的项目指定不同的 `Node` 版本：

![](https://raw.githubusercontent.com/xcom1057136457/DrawingBed/main/20250102101710.png)

找到自己需要的版本进行安装：

![](https://raw.githubusercontent.com/xcom1057136457/DrawingBed/main/20250102101730.png)

![](https://raw.githubusercontent.com/xcom1057136457/DrawingBed/main/20250102101740.png)

可实时查看下载进度，或者取消下载。

安装好之后，点击 **应用** 按钮即可设置成当前的 `node` 版本：

![](https://raw.githubusercontent.com/xcom1057136457/DrawingBed/main/20250102101815.png)

重新打开终端之后，可通过 `node --version & npm --version` 查看是否生效。

## 其他功能

### 命令行工具

`nvmd` 允许您通过命令行快速管理不同版本的 `Node`（`nvmd` 不提供 `Node` 的下载安装功能，如果您需要下载安装新版本的 `Node`，请打开 `nvm-desktop` 客户端）：

```` shell
$ nvmd use 18.17.1
Now using node v18.17.1
$ node -v
v18.17.1
$ nvmd use v20.5.1 --project
Now using node v20.5.1
$ node -v
v20.5.1
$ nvmd ls
v20.6.1
v20.5.1 (currently)
v18.17.1
$ nvmd current
v20.5.1
````

**`nvmd --help：`**

```` shell
$ nvmd --help
nvmd (2.2.0)
The1111mp@outlook.com
command tools for nvm-desktop

Usage: nvmd [COMMAND]

Commands:
  current  Get the currently used version
  list     List the all installed versions of Node.js
  ls       List the all installed versions of Node.js
  use      Use the installed version of Node.js (default is global)
  which    Get the path to the executable to where Node.js was installed
  help     Print this message or the help of the given subcommand(s)

Options:
  -h, --help     Print help
  -V, --version  Print version

Please download new version of Node.js in nvm-desktop.
````

在你通过 `nvmd use` 命令行切换 `Node` 版本之后，请点击刷新按钮让 `nvm-desktop` 同步最新的数据。

更多详情请查看此文档: [`command-tools-intro`](https://github.com/1111mp/nvmd-command#command-tools-intro)。

### 为项目单独设置 Node 版本

不依赖操作系统的功能和 `shell`，完美支持为项目切换不同的 `Node` 版本，而且切换之后不需要重启终端。如果想要体验这个功能，请安装 `Release v2.0.0` 版本。

[`nvmd-desktop Download Page (GitHub Release v2.0.0)`](https://github.com/1111mp/nvm-desktop/releases/tag/v2.0.0)

![](https://raw.githubusercontent.com/xcom1057136457/DrawingBed/main/20250102102500.png)

点击 **添加项目** 按钮，选择项目目录，然后为项目选择需要的 `Node` 的版本（已安装）。

选择版本之后，在你的项目的跟目录中会添加一个 `.nvmdrc` 文件，内容为选择的版本号，`nvm-desktop` 通过此文件为终端设置 `Node` 的版本。重新打开你的终端进入到项目目录下，通过 `node --version` 查看是否生效。

全局设置的 `Node` 版本为 `v18.17.1`：

![](https://raw.githubusercontent.com/xcom1057136457/DrawingBed/main/20250102102613.png)

为项目指定 `v20.5.0` 版本：

![](https://raw.githubusercontent.com/xcom1057136457/DrawingBed/main/20250102102638.png)

终端该项目根目录下查看：

![](https://raw.githubusercontent.com/xcom1057136457/DrawingBed/main/20250102102653.png)

更多说明请查看另一篇文章文章：[Node 版本管理工具：nvm-desktop 进化啦！不依赖操作系统的功能和 shell，完美支持为项目切换不同的 Node 版本](https://juejin.cn/post/7276716177119772733)

### 通过系统托盘菜单快捷管理 node 版本

避免频繁打开界面窗口，通过菜单栏选项快捷切换 `node` 版本

#### `Macos`:

![](https://raw.githubusercontent.com/xcom1057136457/DrawingBed/main/20250102102806.png)

#### `Windows`:

![](https://raw.githubusercontent.com/xcom1057136457/DrawingBed/main/20250102102845.png)

### 同步最新发布的 `node` 版本

因为 `node` 所有版本信息数据默认会做缓存处理，所以如果想要查看最新发布的 `node` 版本，点击 **远程刷新** 按钮同步最新数据即可：

可通过官方链接查看：[`https://nodejs.org/dist/index.json`](https://nodejs.org/dist/index.json)

![](https://raw.githubusercontent.com/xcom1057136457/DrawingBed/main/20250102103025.png)

### 自定义下载镜像地址

默认下载镜像地址是：[`nodejs.org/dist`](https://nodejs.org/dist)，如果你所在的地区下载速度慢，那么你可以更改适合你自己的下载地址以快速下载。

推荐地址：[`https://npmmirror.com/mirrors/node`](https://npmmirror.com/mirrors/node)

![](https://raw.githubusercontent.com/xcom1057136457/DrawingBed/main/20250102103144.png)

### 多语言和多主题

多语言目前支持 **`English`** 和 **简体中文**：

![](https://raw.githubusercontent.com/xcom1057136457/DrawingBed/main/20250102103223.png)

主题目前支持 **跟随系统** 、 **亮色** 和 **暗黑** 三种模式：

![](https://raw.githubusercontent.com/xcom1057136457/DrawingBed/main/20250102103304.png)

### 已安装界面

在已安装界面快速找到已经安装和当前使用的 `node` 的版本，方便切换和管理。

![](https://raw.githubusercontent.com/xcom1057136457/DrawingBed/main/20250102103340.png)

### 查看 `node` 发布的更新日志

在主界面通过点击 `node` 的版本号，可跳转至官方发布的对应版本的 `Changelog` 界面（随时掌握新版本的动向）：

![](https://raw.githubusercontent.com/xcom1057136457/DrawingBed/main/20250102103430.png)

