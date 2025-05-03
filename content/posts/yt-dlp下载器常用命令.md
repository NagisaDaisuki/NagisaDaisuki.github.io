+++
date = '2025-05-04T02:48:37+08:00'
draft = false
title = 'Yt Dlp下载器常用命令'
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
