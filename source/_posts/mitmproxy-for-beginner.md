title: mitmproxy 入门案例 -『抱抱』24小时销毁的真相
date: 2015-05-27 23:35:37
tags:
- mitmproxy
- 抱抱
categories: 技术教程
---

<blockquote class="blockquote-center">真相永远只有一个!
-- 江戶川柯南</blockquote>

<img src="http://ww3.sinaimg.cn/large/6273fe87gw1esjy6qdq4mj21300mjgsk.jpg" class="full-image" />

自称『新一代社交软件』的『抱抱』，被很多人称为又一个『陌陌』。
虽然阅后即焚、匿名社交已经不是什么新鲜概念了，但是作为一名产品狗，自从 Snapchat 被爆出裸照丑闻之后，我就一直很关注这些以阅后即焚为卖点的APP的安全性和对用户隐私的保护措施。
是否真的阅后即焚？
是否真的没有获取用户身份信息？
是否真的匿名？

本教程将通过 mitmproxy 最基本的使用方法，带你还原事实的真相。
- **难度指数：** 新手入门
- **使用工具：** [mitmproxy](https://mitmproxy.org/)
- **示例系统：** Mac
- **适合人群：** 比如像我这样的产品狗

<!-- more -->

## 安装并启动 mitmproxy
1. 安装 mitmproxy
```bash
pip install mitmproxy
```

2. 启动 mitmproxy
```bash
mitmproxy
# 端口默认为 8080，如果你有程序已占用此端口可以指定其他端口(如1234)
mitmproxy -p 1234
```

## 配置手机端
**PS:** 这里以 iPhone 为例，其他平台手机请自行搜索不再赘述。

测试手机应和运行 mitmproxy 的电脑同处一个网络，手机网络设置代理如下：

![](http://ww3.sinaimg.cn/large/6273fe87gw1esk0cwz77fj20ar0ioabo.jpg)

## 监听网络
手机代理设置完毕后，这时候就可以打开『抱抱』了。
随便把玩几下，回到电脑终端看看是不是出现了很多信息，这时候我们需要过滤一下网络请求，
因为我们只关心『抱抱』请求的网络，其他 APP 的我们不管。

```bash
l # 设置 limit filter
myhug.cn 回车
```
现在只有 url 里包含 `myhug.cn` 的请求才会显示出来。

现在可以开始我们的实验了，
我想知道『抱抱』所说的发出的图片会在24小时（或者其他设定的时间）自动销毁是不是真的销毁。

我发了一张图，设定为10分钟销毁

![](http://ww1.sinaimg.cn/large/6273fe87gw1esk122443qj20ak0iutbg.jpg)

然后在 mitmproxy 里找到这个图片的地址（一般会在最底下的几个请求里）

![](http://ww2.sinaimg.cn/large/6273fe87gw1esk0zat84ij20j209678k.jpg)

我们在浏览器输入图片地址，看看是不是相同的图片

![](http://ww3.sinaimg.cn/large/6273fe87gw1esk167tjuwj20h00j7wjt.jpg)

确定是这张图片后，我们就等个10分钟吧，真相马上就会出现

![](http://ww1.sinaimg.cn/large/6273fe87gw1esk19iw5x8j20ao0iracs.jpg)

10分钟后，手机里『显示』已销毁，并且在发布的历史里也找不到了刚刚那张图。这对于普通用户来说，手机里已经不存在的图片，确实可以说是『已销毁』。
但事实上，这张图片并没有在服务器里删掉，你可以在浏览器里重新输入刚刚那个图片地址，[十分钟销毁](http://puj.myhug.cn/pic/w/j94af5e7d64968a00e5f2e109!wbig)，是不是还可以看得到？

没有删掉服务器上的图片，只是不在手机端显示，『抱抱』这是葫芦里卖什么药？
也许只是浏览器缓存？我想。不是。
也许他们工程师设了定期任务要定期才清理？我想。
也许是吧，不过距离我上次试验已经过去了4天了，依然没发现图片被删除。

『抱抱』24小时销毁的真相已经很清楚了，销毁并不是真正的销毁，别有用心的人不费吹灰之力（mitmproxy 可以跑脚本）就可以拿到所有图片的地址并保存下来，如果真的有人盯上的话，下一个泄露风波可能已经不远了。

姑娘们汉子们，流氓不可怕，最怕流氓有文化，请谨慎。




