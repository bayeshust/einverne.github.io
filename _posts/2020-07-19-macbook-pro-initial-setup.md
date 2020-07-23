---
layout: post
title: "MacBook Pro 初始设置记录"
tagline: ""
description: ""
category: 学习笔记
tags: [unix, macos, setup, github,]
last_updated:
---

这里就简单的记录一下我从 Linux Mint 迁移到 MacOS 根据我的个人需求来初始化新的 MacBook Pro 的一些设置，和一些基本的感想。下面的内容会按照我自身的需求出发，我会列举我想要的功能然后在此基础上我需要借助哪些工具来实现。在切换到 MacBook Pro 之前，我使用了大约 6 年多的 Linux Mint，我已经有一套我自己的 Workflow，在切换到 Mac OS 之前我就在想哪一些的事情我是必须有 Mac 的软硬件才能做到，并且很提高某一方面的效率的，我列了一些

- 被很多人追捧的触摸板，当然这个是硬件软件的结合其他系统可以追赶但体验确实不如 Mac 完整
- 一些无法在 Linux 上跑起来的应用，一些基本工具应用，Adobe 系列的软件主要是 Lightroom


## 基础设置
首先是一些必要设置的设置，后面的一切都依赖这些设置。

### 设置代理
国内的网络环境，这已经成了所有设置的基础，甚至我想先下载一个 Chrome 都需要依赖代理设置好。首先从 GitHub 下载 [ClashX](https://github.com/yichengchen/clashX/releases)，然后导入 v2ray 配置。（或者可以用 v2rayU)

ClashX 的配置文件在 `~/.config/clash/` 目录下。

在终端中要进行代理：

    # 设置 socks5 代理
    export http_proxy="socks5://127.0.0.1:1080"

alias

     alias setproxy="export ALL_PROXY=socks5://127.0.0.1:1080" alias unsetproxy="unset ALL_PROXY"

或者设置 curl (curl >= 7.21.7) 代理：

	curl -x socks5h://localhost:1080 http://www.google.com/

### 安装浏览器
在其他系统上在浏览器中的时间伴随着我使用系统的时间，大量的事是在浏览器的页面中完成的，比如在 Trello 中记录时间，比如收发邮件，比如密码管理等等。

有了代理，就可以很快的安装上 Google Chrome，然后登陆账号，没几分钟我所有的同步设置就全部来了，包括书签，插件，甚至历史记录。

LastPass 插件登录，密码同步；Trello 登录，代办事项同步；Tampermonkey 需要到设置中开启高阶设置，然后启用浏览器同步，所有的 userscripts 也同步过来了。等等还有很多的插件也一并同步了。Chrome 此时就已经可以是日常使用了。

### 输入法

我使用小鹤双拼，在开机设置的时候我就已经选择了双拼，然后在输入法中简单的设置一下使用小鹤双拼即可。在 Mac 下使用 <kbd>Ctrl</kbd>+<kbd>Space</kbd> 来切换输入法。

系统自带的小鹤双拼用起来似乎还没遇到什么大问题，先不配置 Mac 下的 [Rime](/post/2014/11/rime.html) 了。如果需要编译安装 [Squirrel](https://github.com/rime/squirrel/blob/master/INSTALL.md) 可以按照这个文档安装。

![double-pinyin-flypy.png](/assets/double-pinyin-flypy.png)

### 开启 SSH 远程登录
在系统设置 Sharing 中需要开启远程登录，这样就可以通过 `ssh yourname@<mac.ip>` 来登录系统了，非常方便我在局域网中将我原来的配置以及文件通过 rsync 传输过来。

### Mac 上的包管理 homebrew
Linux Mint 上继承了 Debian/Ubuntu 系列的 apt 包管理工具，一些工具的安装非常方便，Mac 下有 homebrew。

[官网](https://brew.sh/) 下载安装即可，如果遇到说 curl 443 端口连接不上，那就只能先 Chrome 上把这个脚本下载下来，然后 `bash install.sh` 来手动安装了。

homebrew 也还提供了字体安装的支持，安装 `homebrew-cask-fonts`

```
brew tap homebrew/cask-fonts                  # tap 类似于 apt 中添加第三方源的 add repository
brew cask install font-inconsolata
brew cask install font-fira-code
brew cask install font-source-code-pro

brew tap homebrew/cask-drivers
```

然后配置国内镜像，加速下载。

- <https://mirrors.tuna.tsinghua.edu.cn/help/homebrew/>

brew 常用的命令：

```
brew search package_name
brew info package_name
brew install package_name
brew update
brew upgrade package_name
brew uninstall package_name
brew list
brew config
brew doctor
brew uninstall package_name
```

所有的 `brew cask` 包，可以到[这里](https://formulae.brew.sh/cask/) 查看。

homebrew 会将软件安装到 `/usr/local/Cellar`，然后通过软链接链接到 `/usr/loca/bin` 目录。

```
brew install asdf tree tmux hub p7zip openssl curl node automake autoconf ack
brew install macvim --with-cscope --with-override-system-vim --with-lua --HEAD
brew install coreutils curl git asdf
rehash
```

brew 和 `brew cask` 的区别在于 cask 用来安装 GUI 软件。

brew cask

```
brew cask install anki alfred eudic iterm2 visual-studio-code
```

brew 的备份和恢复，如果要在两台 Mac 间备份和恢复 brew 安装的包，可以使用：

	brew bundle dump    # dump
    brew bundle         # 恢复

`brew bundle dump` 会生成一个 Brewfile 文件，这是一个纯文本的文件，里面列举了系统上的 brew 配置和安装的列表，那么我只需要维护一个 Brewfile 文件就可以一键安装必备的命令和桌面软件了。

通过 App Store 安装的应用可以通过 mas 来管理。

### 手势
一直都说 Mac 的手势领先其他厂商，个人初使用来看确实非常顺手，没有卡顿，并且大面积的触摸板使用体验确实不错。

除了常用的双指上下滚动，左右翻页还有这样一些操作：

- 三指向上，Mission Control，会显示当前所有打开的窗口
- 三指向下，可以显示同一个应用的不同窗口
- 三指左右，可以在全屏的应用间切换
- 四指捏合，显示 Launchpad
- 四指放开，显示桌面

但是关于选择页面中的文本在 Mac 就比较有趣了，我一般的行为模式就是用左手大拇指双击进入选择模式，然后接触触摸板选择文字释放左手大拇指。我这个操作习惯在 Mac 需要用力按下触摸板选择，后来在 System Perferences -> Accessibility -> Mouse & Trackpad -> Trackpad Options -> Enable dragging without Drag Lock 找到了设置。

这个选项里面有三个选择：

- without drag lock
- with drag lock
- three finger drag

很多人推荐开启第三个，我个人不太喜欢，明明能有用一个指头完成的事情为何要用三个指头。

但前两个就有点暧昧不清了，这里具体解释一下：[^drag]

[^drag]: <https://apple.stackexchange.com/a/7195/149497>

- without drag lock: 这个模式下的拖拽选择操作是这样的：1. Double tap 然后第二次 Tap 不放开，直接开始拖拽，然后放开；2. 然后根据最后停顿的时间来判断是否结束选择，如果是短暂的停顿则即使松开手指也会继续在选择模式，直到再 Tap 一下触摸板，而如果在选择时有较长的停顿，操作系统则认为选择结束
- with drag lock: 则是 Double Tap 然后不放开手指进入选择模式，松开手指也会继续在选择模式中，直到再 Tap 一下停止选择。

我个人的习惯还是选择了第一个 without drag lock.


## 编程开发工具

### iTerm

[官网](https://www.iterm2.com/) 下载安装即可。

配置和 Guake 类似的下拉显示。

zsh, vim, tmux 的配置放在 [dotfiles](https://github.com/einverne/dotfiles) 项目管理。

	git clone git@github.com:einverne/dotfiles.git
	ln -s ~/dotfiles/.zshrc ~/.zshrc
	source ~/.zshrc

    ln -s ~/dotfiles/.vimrc ~/.vimrc
	# 然后进入 Vim，执行 `:PlugInstall`
	ln -s ~/dotfiles/tmux/.tmux.conf ~/.tmux.conf
	ln -s ~/dotfiles/tmux/.tmux.conf.local ~/.tmux.conf.local

### JetBrains Toolbox
下载 JetBrains Toolbox，然后一个个选择想用的 IDE。

### SmartGit
[官网](https://www.syntevo.com/smartgit/download/) 下载，我个人非常喜欢的一个 Git 客户端，跨平台，页面也很清晰。不过简单的提交和使用 IntelliJ IDEA 自带的 Git 管理也已经足够好了。


### 设置 GitHub SSH Key
设置 GitHub 的 SSH Key。

    ssh-keygen -t rsa -b 4096 -C "your_email@example.com"

然后 clone 我的 dotfiles 配置，笔记，wiki 等等配置。



## 默认软件的熟悉


### Finder 中显示 Home 目录


使用快捷键 Command+Shift+H


## 常用快捷键

快捷键      | 解释
----------|-----------
command + 1 左边的按键 | 在同一个应用不同窗口间切换
Shift+command+3 | 截取全屏
Shift+command+4 | 截取部分，通过光标选择
Shift+command+5 | 截取部分，有更多选项

Mac 全部快捷键

- <https://support.apple.com/en-us/HT201236>

## 其他需求

### 压缩图片的需求
在 Mac 上随便截一张图可能就是 7，8MB 大小，非常不适合在互联网上分享，我一般会使用 Google 的 [Squoosh](https://squoosh.app/) 来进行压缩。

有些人推荐 squash 这款软件，简单的看了一下官网，似乎就我这个需求并不需要 Squash 这款软件。

### 多重粘贴板
在 Linux 上我使用 fcitx 自带的粘贴板 <kbd>Shift</kbd>+<kbd>;</kbd> 就可以呼出，因为就是输入法的功能，所以非常方便。在 Windows 下用 Ditto 这样一款软件。那么切换到 MacOS 就想要一个代替品。

    brew cask install copyq

### 在文件管理器 Finder 中快速打开终端
借助 [OpenInTerminal](https://github.com/Ji4n1ng/OpenInTerminal) 这个 Finder 的插件可以实现在 Finder 目录中，立即打开终端。

借助这个插件还可以：

- 选中的文件夹，通过 Vim，或者其他编辑器直接打开
- 快速复制当前的路径

这里不得不吐槽一下，文件管理器，Mint 中的 nemo 使用起来要舒服多，这上面的操作，右键就能完成，完全不需要额外安装插件。

### 改键的需求
在 Mint 我已经有了一套熟悉的快捷键，现在到了 MacOS 上一边熟悉自带的快捷键，也想对默认的一些快捷键做修改，这个时候就需要 Karabiner-Elements。

Karabiner-Elements 比较强大，可以自定义修改几乎键盘上的每一个键，不过我个人并不推荐把键盘修改的面目全非，这需要花费很长的时间来适应。

Karabiner-Elements

- <https://github.com/pqrs-org/Karabiner-Elements>


### Telegram 即时聊天工具
去 Telegram 官网看，发现 MacOS 下有两个客户端，一个叫做 Telegram Desktop，这个和 Windows 和 Linux 放在一起；另一个叫做 Telegram for MacOS，简单了解一下后，发现这个客户端是单独用 Mac 的原生语言实现。这两个的区别在于 Telegram Desktop 使用跨平台的实现，所以体验上和其他两个平台相似，原生实现的 Telegram for MacOS 则提供了加密等额外的功能。


### 文件同步需求
从 Dropbox 换成了 分布式的 [Syncthing](https://syncthing.net/downloads/).

Syncthing 的配置[设置](https://docs.syncthing.net/users/config.html) 在 `$HOME/Library/Application Support/Syncthing`.


另外也会用中心化的 [NextCloud](https://nextcloud.com/install/#install-clients) 作为备份。

### 播放器需求
开源的选择 IINA

- [官网](https://iina.io/)

或者老牌的 VLC


### 笔记的需求
历史的笔记在为知笔记里面，下载，登录数据就回来了。

- [官网](https://www.wiz.cn/wiznote-mac.html) 下载。

WizNote 打开的时候显示不被认证的开发者，需要执行

	sudo spctl --master-disable

开启信任任何来源的安装，当然这个操作会降低系统的安全性，谨慎！

另外今年起，我渐渐的将笔记迁移到了 Obdisian

- [官网](https://obsidian.md/) 下载。

我在 GitHub 上新建了一个 Private 的项目来同步 Obsidian 的笔记。


### 听音乐的需求
唯一的选择，网易云音乐，我之前也写过文章，在体验过市面上所有的音乐软件后，选择了网易，然后已经很多很多年了。



### Dash 文档查看
[官网](https://kapeli.com/dash) 下载安装。

### 窗口管理
在 Mac 上我发现我无法想在 Mint 中拖拽窗口到左右边缘，然后将窗口左右分屏，导致现在 Mac 上的窗口层层叠叠，管理起来非常的不方便。找了几个开源的工具。

- SizeUp
- [Spectacle](https://www.spectacleapp.com/)
- [Rectangle](https://rectangleapp.com/)

体验了一下之后选择了 Rectangle，虽然也看到了 MacOS 上的平铺窗口管理。

- [Amethsy](https://github.com/ianyh/Amethyst)
- chunkwm

但这些都需要大量的定制，所以先放着了。

### 图片预览及管理
[Pixea](https://apps.apple.com/cn/app/pixea/id1507782672)
一款优秀的看图软件。

### 录制 GIF
LICEcap

### 压缩解压缩

- https://www.keka.io/en/


### Tiling Windows Manager

yabai

- <https://github.com/koekeishiya/yabai>

### 查看监听的端口
Mac 上使用 netstat 显示监听的端口：

    netstat -an | grep LISTEN

或者使用 lsof:

    sudo lsof -iTCP -sTCP:LISTEN -n -P

lsof 可以看到具体某个端口关联的 PID。

### 安装 Java 开发环境



### 安装 Python 环境

## 迁移 Lightroom 图片库
我的很大一部分照片库在 Windows 的 Lightroom 中，幸亏 Lightroom 的迁移并不麻烦，在 Windows 上使用 WinSCP，然后局域网连上 Mac，直接将 Lightroom 所在的 Pictures 图片库复制到 Mac 上面的图片库中，然后将 Lightroom 的 `.lrcat` catalog 文件夹也拷贝到 Mac 上，这个目录可以在 Windows 的“编辑”-“目录设置”中找到。

拷贝完成后在 Mac 上打开 lrcat 文件，然后在 Lightroom 中右击文件夹，Find missing folder，重新导入就 OK 了。

## 神奇操作

### Mac 上 Chrome 遇到 NET::ERR_CERT_INVALID
我使用的 Resilio Sync 的后台管理界面使用了 BitTorrent 自己签发的证书，所以 Chrome 中打开的时候会报错“NET::ERR_CERT_INVALID”，普通情况下我知道该风险，在其他操作系统中会提供一个选项，点击高级可以继续访问链接，但是在 Mac 上并没有这个按钮，我搜索了一下，想要尝试信任证书，无果，后来发现一个神奇的方法，在页面中点击空白处，然后输入：**thisisunsafe**，即可。略神奇。


## reference

- <https://github.com/macdao/ocds-guide-to-setting-up-mac>
- <https://github.com/nikitavoloboev/my-mac-os>
- <https://wsgzao.github.io/post/homebrew/>
