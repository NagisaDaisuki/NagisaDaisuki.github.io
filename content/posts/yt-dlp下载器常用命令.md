+++
date = '2025-09-09T14:42:37+08:00'
draft = false
title = 'yt-dlp下载器常用命令'
categories = ["工具"]
tags = ["yt-dlp", "ffmpeg"]
+++


# 🐭 Bilibili 视频下载
> ✅ 支持大多数公开视频（包括番剧、电影等）
> 
> 🚫 暂不支持大会员付费内容

## 🎥 下载B站视频
~~~powershell
yt-dlp "https://www.bilibili.com/video/BV1xxxxxxx"
~~~
### 🎞️ 下载分P视频（只下第1P）
~~~powershell 
yt-dlp --playlist-items 1 "https://www.bilibili.com/video/BV1xxxxxxx"
~~~
### 🎧 下载音频（提取）
~~~powershell
yt-dlp -x --audio-format mp3 "https://www.bilibili.com/video/BV1xxxxxxx"
~~~
 

## 🐦 Twitter 视频下载
> ✅ 支持公开视频和GIF（私密/限制访问的可能无法下载）
### 🎥 下载推文中的视频
~~~powershell 
yt-dlp "https://twitter.com/用户名/status/1234567890123456789"
~~~
### 🚀 下载最高画质
~~~powershell 
yt-dlp -f best "https://twitter.com/用户名/status/1234567890123456789"
~~~
## 📺 YouTube 视频下载
### 🎥 下载最高画质（视频 + 音频 合并）
~~~powershell 
yt-dlp -f bestvideo+bestaudio "https://www.youtube.com/watch?v=xxxxxxx"
~~~
### 🎧 下载为MP3
~~~powershell
yt-dlp -x --audio-format mp3 "https://www.youtube.com/watch?v=xxxxxxx"
~~~


## 使用`-F(--list-formats)`和`-f(--format)`选项列出视频所有可下载格式(搭配cookies下载网站最高画质视频和音频)
1. **查看可用格式**:
~~~shell
yt-dlp -F --cookies /path/to/your/cookies "https://www.bilibili.com/video/BVxxxxxx"
~~~

执行这个命令后会看到一个列表，记住`ID`和对应的视频的`RESOLUTION`

2. **指定格式并下载**:（该加cookies的加cookies）
- **只下载视频**: 
~~~shell
yt-dlp -f <视频ID> "https://www.bilibili.com/video/BVxxxxxx"
# 假如1080p视频id为600
yt-dlp -f 600 "https://www.bilibili.com/video/BVxxxxxx"
~~~

- **只下载音频**:
~~~shell
yt-dlp -f <音频ID> "https://www.bilibili.com/video/BVxxxxxx"
# 假如最高音质的音频id为125
yt-dlp -f 125 "https://www.bilibili.com/video/BVxxxxxx"
~~~

- **同时下载视频和音频**:
~~~shell
yt-dlp -f 600+125 "https://www.bilibili.com/video/BVxxxxxx"
~~~

- **下载最佳视频和最佳音频**:
~~~shell
yt-dlp -f bestvideo+bestaudio "https://www.bilibili.com/video/BVxxxxxx"
yt-dlp -f bv+ba "https://www.bilibili.com/video/BVxxxxxx"
~~~

