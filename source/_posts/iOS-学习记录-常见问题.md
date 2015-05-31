title: iOS 学习记录 - 常见问题 (更新中)
date: 2015-05-27 23:08:40
tags: iOS
categories: 学习记录
---

<blockquote class="blockquote-center">能用 💰 解决的问题，都不叫问题，
  能用 ⌚️ 解决的问题，都不叫问题。
</blockquote>

## 1. Swift / Objective-C 混合使用
- [How to call Objective C code from Swift](http://stackoverflow.com/questions/24002369/how-to-call-objective-c-code-from-swift)
- [How to use Swift in Objective-C projects](https://medium.com/ios-apprentice/using-swift-in-objective-c-projects-f7e7a09f8be)
- [Mix and Match](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/BuildingCocoaApps/MixandMatch.html)

## 2. Swift/OC 混用，快速创建桥接头文件
要在 Swift 项目中使用 Objective-C，需要一个`Objective-C Bridging Header`文件。
最快的创建方法是新建一个 Objective-C 文件，然后 Xcode 就会弹出一个对话框问你是否创建一个头文件，确认，然后再删掉这个没用的 Objective-C 文件即可。

**Ref：**[快速创建一个桥接头文件](http://happy-coding.org/swift-and-cocoapods/)

## 3. 如何打开 Swift REPL?
Xcode6 Beta 和 Xcode 6.0 和 Xcode 6.1 打开的方法都不一样，最新解答可以跟踪[SF](http://stackoverflow.com/questions/24011120/how-can-i-use-swift-in-terminal)

```
sudo xcode-select -switch /Applications/Xcode6-Beta.app/Contents/Developer
xcrun swift 或者 lldb --repl

```

<!-- more -->

## 4. 如何在 Simulator 的 Photo library 添加图片和视频？
- 图片：直接把图片拖到模拟器就好
- 视频：直接把视频拖到模拟器，这时候 Safari 会自动打开并播放视频，点击DONE，然后从『分享』图标里找到『Save Video』

## 5.
