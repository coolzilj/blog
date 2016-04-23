title: 视频直播这么火，咱来做个直播 APP 吧
date: 2015-10-02 17:38:11
tags:
- rtmp
- hls
- nginx
- ffmpeg
categories: 学习记录
---

## 1. 安装配置 rtmp server

### 安装 nginx 以及 nginx-rtmp-module
以 MacOS（Yosemite） 为例,
```
brew uninstall nginx
brew tap nginx-full
brew options nginx-full
brew install nginx-full --with-rtmp-module
```

### nginx
配置位置: `/usr/local/etc/nginx`
启动： `sudo nginx` （如果 pid 文件为空可以执行这个命令先）
重启： `sudo nginx -s reload`
查看编译参数： `nginx -V`

<!-- more -->

#### rtmp-nginx-module 位置
`/usr/local/share/rtmp-nginx-module/`
自带一个使用例子，在 test/www 里
自带一个 nginx.conf 示例文件
往 `www` 文件夹里添加一个播放 hls 页面，

``` playhls.html
<!DOCTYPE html>
<html>
<head>
    <title>HLS Player</title>
</head>
<body>
<video height="270" width="480" controls>
    <source src="http://ip:port/hls/mystream.m3u8" type="application/vnd.apple.mpegurl" />
    <p class="warning">Your browser does not support HTML5 video.</p>
</video>
</body>
</html>
```

### ffmpeg

#### 摄像头作为 rtmp 源
```
ffmpeg -f avfoundation -i 0 -c:v libx264 -an -f flv rtmp://localhost/myapp/mystream
```
切片, 如果出现 Connection to tcp://localhost:1935 failed (Connection refused) 错误
尝试重启 sudo nginx -s reload
```
ffmpeg -f avfoundation -i 0 -c:v libx264 -an -f flv rtmp://localhost/hls/mystream
```

#### MP4 文件作为 rtmp 源 (切片)
```
ffmpeg -re -i ~/Movies/MV/UP\&DOWN.mp4 -c copy -f flv rtmp://localhost/myapp/mystream
ffmpeg -re -i ~/Movies/MV/UP\&DOWN.mp4 -c copy -f flv rtmp://localhost/hls/mystream
```

#### 查看 MacOS 上的输入设备
```
ffmpeg -f avfoundation -list_devices true -i ""
```

### 切片文件位置
/tmp/app


## 2. iPhone 作为 rtmp 源
PLCameraStreamingKit 修改为本地的 rtmp 服务器
pushURL: rtmp://localip/myapp/mystream 或者 rtmp://localip/hls/mystream

### iPhone 播放
- hls ios 支持直接播放
- rtmp https://github.com/Bilibili/ijkplayer

## 3. 延迟
切片 hls：26s 延迟
不切片 rtmp: 3s 延迟

## 4. 参考资料
- [](http://anotheren.com/62)

## 实战

### Dockerfile

Dockerfile: https://github.com/awakening-church/awakening-nginx-rtmp

启动：
```
docker run -e PUBLISH_SECRET=hahaha -e CORS_HTTP_ORIGIN='(https?://[^/]*\.liujin\.me(:[0-9]+)?)' -p 80:80 -p 1935:1935 awakening/awakening-nginx-rtmp
```

测试 push url：
```
ffmpeg -re -i ~/Movies/MV/UP\&DOWN.mp4 -c copy -f flv rtmp://localip/pub_hahaha/1
```

ssh container 查看：
```
docker exec -i -t focused_panini(container name) bash
```
