title: Mac 常用技巧 - 工具篇 (更新中)
date: 2015-05-26 14:12:23
tags: Mac
categories: 学习记录
---
<blockquote class="blockquote-center">
  工欲善其事，必先利其器。
  《论语·卫灵公》
</blockquote>

## 软件管理

1. [Homebrew](http://brew.sh/)
当所有工具都可以用同样一句命令来安装，世界是不是变得和蔼可亲了很多？
情景一：今天想学学 Go 来玩玩。恩，`brew install go`
情景二：github 发现了一个好玩的项目，项目需要 mongodb。恩，`brew install mongodb`
...
更多情景请自行脑补，一句话概括，Homebrew 让你一键安装全世界，
洁癖和强迫症患者强烈推荐。

2. [Cakebrew](https://www.cakebrew.com/)
如果你用不习惯命令行，Homebrew 这么吊的神器当然还有 GUI 啦。
Cakebrew 就是 The Mac App for Homebrew,
<img src="http://ww2.sinaimg.cn/large/6273fe87gw1eshnbqqpz3j20iy0ejdhs.jpg" class="full-image" />

3. Homebrew Cask
作为 Homebrew 的补充，Cask 让你告别 .dmg, .pkg 安装程序文件。
安装 Chrome, `brew cask install google-chrome`.


## 编程工具

1. pip
pyton 的包管理工具，就像`ruby`的`gem`
安装方法：`sudo easy_install pip`

2. [n](https://github.com/tj/n)
node.js 版本管理工具

3. [rvm](https://rvm.io/)
ruby 的版本管理工具
