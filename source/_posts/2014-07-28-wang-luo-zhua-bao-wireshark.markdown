---
layout: post
title: "网络抓包wireshark"
date: 2014-07-28 10:33:35 +0000
comments: true
categories: 
---
##抓取youku
先使用wireshark抓取了一次youku申请播放的过程
####过滤字符串
在Filter里输入：http contains "getPlayList"
####过滤ip
ip.src==101.227.10.18 or ip.dst==101.227.10.18
####复制数据包内容
选择数据包，右键--copy
####youku实际地址
http contains "getFlvPath"      
![结果](/images/cap_youku1.jpg)
在vlc中打开路径可以播放：       

