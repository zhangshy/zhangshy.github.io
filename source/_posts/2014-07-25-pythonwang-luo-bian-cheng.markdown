---
layout: post
title: "python网络编程"
date: 2014-07-25 17:03:38 +0000
comments: true
categories: python
---

###获取url数据
    url = "http://www.youku.com/"
    req = urllib.request.Request(url)
    req.add_header('User-agent', 'Mozilla/5.0')
    r = urllib.request.urlopen(req)
    print(r.read().decode("UTF-8"))

###保存图片
    urllib.request.urlretrieve(url, fileName);  #将url内容存入文件
###分析html
使用html.parser.HTMLParser分析html
