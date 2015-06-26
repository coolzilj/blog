title: Docker + DigitalOcean + Shadowsocks 5分钟科学上网
date: 2015-05-27 13:04:14
tags:
- 科学上网
- Docker
- DigitalOcean
- Shadowsocks
categories: 技术教程
---

<blockquote class="blockquote-center">春色满园关不住，一枝红杏出墙来。
  -- 科学上网办
</blockquote>

5分钟？就能科学上网？！！！！
有人肯定要说我标题党了，
如果你已经有一个 DigitalOcean(以下简称 DO) 账号或者 一个 VPS，
5 分钟已经算多了。
不信你自己掐表算，不废话，上教程。

<!-- more -->

**PS:**
- 因为这是给对服务器不熟悉的新手写的教程，像应该创建独立用户而不是使用 root 操作等涉及服务器安全的问题不在讨论范围内。有兴趣的可以自行Google, 因为看完此文你已经能科学上网了。^^
- Q: Shadowsocks 的安装已经足够简单了，为什么要用 Docker？
  A: 再强调一次，写这个教程的时候，我的假想读者是对 OPS 所知甚少、对科学上网有强烈需求、又想自己折腾一番的初级电脑使用者。在一台全新的主机安装 SS？谁能保证安装过程不会出错？而 Docker 开箱即用，出错率更低。当然还有一个原因，我是强迫症，喜欢 Docker 没道理，也想让更多人认识 Docker.

## 你需要准备
1. DO 账号
利益相关：DO 有一个 Referral Program，
用我的小尾巴注册 DO 会马上送你 10 刀，[> 我的小尾巴](https://www.digitalocean.com/?refcode=3f22be5d5073),
10 刀相当于可以免费使用 2 个月，当然你可以无视我。

2. [Shadowsocks for OSX 客户端](https://github.com/shadowsocks/shadowsocks-iOS/wiki/Shadowsocks-for-OSX-%E5%B8%AE%E5%8A%A9)
本文以 Mac 平台客户端为例，SS 有各个平台的客户端 [（传送门）](https://shadowsocks.com/client.html)，具体使用不再赘述，请自行搜索。

更新：有朋友反映 Shadowsocks for OSX 给的下载链接不能访问，看来 Sourceforge 也被认证了，提供一个网盘链接给大家：[ShadowsocksX-2.6.3.dmg 百度网盘](http://pan.baidu.com/s/1qWPyrXq)

## 服务端配置

### 创建一个 Droplet
1. 填上 Droplet 名字
2. 选择一个 Size

  ![](http://ww4.sinaimg.cn/large/6273fe87gw1esiqw092odj20nb0dhdhi.jpg)

3. 选择 Region

  本人亲测，电信用户选择旧金山速度最快最稳定。
  ![](http://ww3.sinaimg.cn/large/6273fe87gw1esir1x330ej20my0abq3l.jpg)
  Ref: [知乎：DigitalOcean 选择 Region 的问题？](http://www.zhihu.com/question/25529727)

4. 选择 Image

  这里选择 Docker
  ![](http://ww1.sinaimg.cn/large/6273fe87gw1esiqzg1zg7j20mw0iaq5o.jpg)

5. 添加 SSH keys (可选)

  不想每次连 SSH 都输密码的建议还是添加，我这里已经添加过就不再详细说明了。
  ![](http://ww4.sinaimg.cn/large/6273fe87gw1esir3f0dppj20n205omxj.jpg)

6. 点击创建，等待一分钟左右，一台已经装有 Docker 的服务器就已经创建成功，顺便记下 ip 地址。

### 安装 Shadowsocks
1. 首先先连上刚刚创建好的主机

  打开『终端』，输入：
  ```bash
  ssh root@ip地址 #没记下 ip 地址的可以去 DO Droplets 页面找到
  ```
  如果你添加了 SSH keys 的话，直接就可以连上不需要输密码，
  如果你没有添加的话，DO 会通过邮件把密码发送给你，你只需要输入密码就可以连接上主机。

  ![](http://ww4.sinaimg.cn/large/6273fe87gw1esirintsx3j20l30bb0w1.jpg)

  看到这个界面的证明你已经连上主机。

2. 接下来安装 Shadowsocks (以下简称 SS)

  现在就是见证 docker 的强大之处的时候了，安装 SS 你只需要：
  ```bash
  docker pull oddrationale/docker-shadowsocks
  ```
  完成后再输入：
  ```bash
  docker run -d -p 1984:1984 oddrationale/docker-shadowsocks -s 0.0.0.0 -p 1984 -k paaassswwword -m aes-256-cfb
  ```
  上述命令中得 `paaassswwword` 就是等下配置客户端需要的密码，你可以换成你自己的密码。

  现在来检查一下 SS 有没有安装成功了：
  ```bash
  docker ps
  ```

  ![](http://ww1.sinaimg.cn/large/6273fe87gw1esirwqntidj219l01uq3z.jpg)

  如果你看到 STATUS 是 up 的话，服务器端已经配置成功，你可以断开 SSH 回到你的本机。
  ```bash
  exit
  ```

  至此服务端就已经配置好了。

## 客户端配置

以 Shadowsocks for MAC 客户端 为例，安装好后添加服务器配置：

![](http://ww1.sinaimg.cn/large/6273fe87gw1esis28f4isj20go0bdt9h.jpg)

填上 ip 地址，端口，密码，密码就是刚刚的 `paaassswwword`，保存后现在回到浏览器，打开 [Youtube](http://youtube.com), 你已经在科学上网了。

Ref: [Shadowsocks for Mac 帮助](https://github.com/shadowsocks/shadowsocks-iOS/wiki/Shadowsocks-for-OSX-%E5%B8%AE%E5%8A%A9)

**大功告成。是不是 5 分钟都多余了？有任何疑问请留言。**
