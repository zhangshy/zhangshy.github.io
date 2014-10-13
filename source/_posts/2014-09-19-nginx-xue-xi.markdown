---
layout: post
title: "nginx 学习"
date: 2014-09-19 01:20:38 +0800
comments: true
categories: 
---

####配置
配置文件：/etc/nginx/nginx.conf，将

    location /images/ {
            root   /data;
    }

加入/etc/nginx/nginx.conf
####启动
sudo systemctl start nginx
####测试
将cap_jar.jpg复制到/data/images/目录下，运行wget http://localhost/images/cap_jar.jpg 可将文件下载下来
