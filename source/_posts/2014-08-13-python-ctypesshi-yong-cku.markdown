---
layout: post
title: "python ctypes使用c库"
date: 2014-08-13 17:15:28 +0000
comments: true
categories: python
---

[我的例子](https://github.com/zhangshy/myTest/blob/master/python/cvsave2Png.py)
###调用c++
c++编写的so库要加上extern "C"，要不然找不到函数
###传递字符串
使用c_char_p(b"xxxx")传递字符串，如果直接用xxx的话，c层只能得到一个字符。

    from ctypes import *
    rpng = cdll.LoadLibrary("/home/zsy/lqbz/java/librpng.so")
    rpng.savejar2png(c_char_p(b"testEn.jar"), c_char_p(b"test.png"), c_char_p(b"cvout.png"))

###python调用shell
[我的例子](https://github.com/zhangshy/myTest/blob/master/python/pushShell.py)      
    os.system("adb push libjni.so /system/lib")
