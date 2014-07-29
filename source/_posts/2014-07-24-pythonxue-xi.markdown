---
layout: post
title: "python学习"
date: 2014-07-24 16:15:52 +0000
comments: true
categories: python
---

###print
格式化输出，可以结合format格式化输出，end=" " 参数可以改变单次print的结束    

    a=10
    b=12
    print("0x{0:02x}+0x{1:x}={2}".format(a, b, a+b))
    print("0x%02x+0x%x=%d" % (a, b, a+b))
    输出相同：0x0a+0xc=22

%% 百分号标记       
%c 字符及其ASCII码      
%s 字符串       
%d 有符号整数(十进制)       
%u 无符号整数(十进制)       
%o 无符号整数(八进制)       
%x 无符号整数(十六进制)     
%X 无符号整数(十六进制大写字符)     
%e 浮点数字(科学计数法)     
%E 浮点数字(科学计数法，用E代替e)       
%f 浮点数字(用小数点符号)       
%g 浮点数字(根据值的大小采用%e或%f)     
%G 浮点数字(类似于%g)       
%p 指针(用十六进制打印值的内存地址)     
%n 存储输出字符的数量放进参数列表的下一个变量中     

###字典

    headers = {'User-agent':'Mozilla/5.0',
           'cache-control':'no-cache',
           'accept':'*/*'}
    for k, v in headers.items():
        print(k, v)

输出

    User-agent Mozilla/5.0
    cache-control no-cache
    accept */*
    
###chr hex ord
ord返回字符的ASCII  print(ord('a')) 输出97      
chr返回字符 print(chr(97))  输出a       
hex返回16进制   print(hex(97))  输出0x61    

    f=open('xxx.ts', 'r', encoding="ISO-8859-1")
    context = f.read()
    buf = [ord(i) for i in context]

###字符串操作
####查找
find和index，使用index未找到字符串会抛出异常，find不抛异常只是返回-1。

    index1 = str.find("xx")
    index2 = str.find("xxx", index1)#从第index1位置开始查找
####复制
str2 = str[index1: index2]
###时间
获取当前时间localtime，格式化时间strftime

    import time
    print(time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()))
    输出2014-07-24 19:13:43

###文件操作
os.access可以判断文件是否存在，os.mkdir创建文件夹

    if not os.access(filePath, os.R_OK):
        print("目录不存在")
        os.mkdir(filePath, mode=0o777)
