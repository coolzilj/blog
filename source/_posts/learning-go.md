title: Go 学习记录 (更新中)
date: 2016-01-14 13:06:31
tags: go
categories: 学习记录
---

<blockquote class="blockquote-center">Go the right way.</blockquote>

## 基础类型

### 1. `string`, `[]byte`, `[]rune` 的区别
rune 是 int32 的 alias, 在 Go 中，字符串是以 UTF-8 为格式进行存储的，在字符串上调用 len 函数，取得的是字符串包含的 byte 的个数

```
s:="←↑→↓"
fmt.Println(len([]rune(s))) => 4
fmt.Println(len([]byte(s))) => 12
fmt.Println(len(s))         => 12

s:="Hello你好←↑→↓"
fmt.Println(len(s))         => 23
fmt.Println(len([]rune(s))) => 11
```

- Go source code is always UTF-8.
- A string holds arbitrary bytes.
- A string literal, absent byte-level escapes, always holds valid UTF-8 sequences.
- Those sequences represent Unicode code points, called runes.
- No guarantee is made in Go that characters in strings are normalized.
- 英文字符串由 uint8 整型数组（字节数组）组成 每个字符对应一个字节 参考ASCII 码
- 一个汉字（在go中就是一个rune类型）可能由三个或者四个字节组成

Ref:
- [Golang 中获取字符个数以及 emoji 表情处理](http://maiyang.github.io/golang/%E5%AD%97%E7%AC%A6%E4%B8%B2/emoji/%E8%A1%A8%E6%83%85/2015/06/16/golang-character-length/)
- [Strings, bytes, runes and characters in Go](https://blog.golang.org/strings)

<!-- more -->

### 2. 包管理
以 wechat-deleted-friends 为例，如果你执行 go run main.go 会提示
```
# command-line-arguments
./main.go:21: undefined: NewWebwx
```
但如果先执行 `go build`, 会生成一个二进制可执行文件，`./wechat-deleted-friends` 一切正常。
所以，如果有些 lib 是只在本项目使用，可以像这个项目这样，把每个 lib package 都写成 package main，然后该干嘛干嘛，最后先 build 再执行准没错，但不能用 go run 调试而已。

还有一种办法，就是把 lib 文件放在文件夹 lib 下，在 main.go 导入
```
import 'lib'
```
即可，go run 可调试, build 后应该同样可以执行
如
```
main.go
mylib/
  mylib.go
```
The simplest way is to use this:
```
import (
    "./mylib"
)
```

Ref: [import package](http://stackoverflow.com/questions/15049903/how-to-use-custom-packages-in-golang)
