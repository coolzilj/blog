title: 115 离线下载命令行工具（批量添加种子/磁力链）
date: 2015-07-29 17:04:18
tags:
- 115
- 离线下载
- 开源项目
categories: 开源项目
---

![screenshot](https://raw.githubusercontent.com/coolzilj/lixian-115/master/screenshots/screenshot.png)

# [Lixian 115 (115 离线下载命令行工具)](https://github.com/coolzilj/lixian-115)

- [x] add multi torrents (批量添加种子文件)
- [x] add multi magnet links (批量添加磁力链)

## Install

```
$ npm install -g lixian-115
```

## Usage

### Login (登录)
```js
lx115 -l
```

### Add torrents (批量添加种子文件)
```js
lx115 -t path/of/torrents/folder（存放 .torrent 文件的文件夹路径）
```

## Contributing

### Build

```js
npm run build
```

### Watch

To watch for changes, build them and run the tests:

```js
npm run watch
```

## License

MIT © [Liu Jin](http://liujin.me)
