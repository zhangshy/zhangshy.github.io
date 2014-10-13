---
layout: post
title: "c解析命令行参数getopt_long"
date: 2014-09-29 10:31:48 +0000
comments: true
categories: c
---

getopt_long支持长选项的命令行参数解析，[我的例子](https://github.com/zhangshy/Image/blob/master/src/image.cpp)     

1.    单个字符，表示选项
2.    单个字符后接一个冒号：表示该选项后必须跟一个参数。参数紧跟在选项后或者以空格隔开。该参数的指针赋给optarg
3.    单个字符后跟两个冒号，表示该选项后可以有参数也可以没有参数。如果有参数，参数必须紧跟在选项后不能以空格隔开。该参数的指针赋给optarg      

