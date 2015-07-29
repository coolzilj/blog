layout: post
title: Electron 实验项目 - 雪球股票助手
date: 2015-06-08 18:16
tags:
- Electron
- 开源项目
categories: 开源项目
---

![](http://ww3.sinaimg.cn/large/6273fe87gw1eswsehpfioj20cu0e0q4l.jpg)

# [雪球股票助手 for Mac Menubar][75e29935]

[Electron](http://electron.atom.io/)(原名 Atom Shell) 是一个原本为 Atom 编辑器设计的，跨平台的应用外壳（Application Shell）。

它将 Chromium 和 Node.js 的事件循环整合到了一起，同时提供了一些与原生系统交互的 API。我们可以通过 Electron，利用熟悉的 Web 技术去构建具有原生体验的跨平台的桌面应用。

『雪球股票助手』因项目依赖 [menubar](https://github.com/maxogden/menubar) 模块，暂时只支持 Mac 平台，有兴趣的同学可以去给 menubar 提交 Windows 和 Linux 的 patch。


## 说明

- 编辑 `config.json`, 修改 `uid` 为你的雪球用户 id
- `npm install` 安装依赖模块
- `npm start` 不经过打包，从命令行直接运行『雪球股票助手.app』
- `npm run build` 打包『雪球股票助手.app』

## TODO

- [x] 排序
- [ ] 压缩打包程序
- [x] 修正涨跌幅 0% 的颜色


  [75e29935]: https://github.com/coolzilj/xueqiu-stock "雪球股票助手"
