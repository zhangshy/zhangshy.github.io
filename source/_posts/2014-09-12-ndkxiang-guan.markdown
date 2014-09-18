---
layout: post
title: "ndk相关"
date: 2014-09-12 23:52:50 +0800
comments: true
categories: 
---

###头文件
在android-ndk-r9c/platforms/android-14/arch-arm/usr/include/下可以查看ndk相关的头文件，比如android/log.h
###编译工具
编译工具在android-ndk-r9c/toolchains/arm-linux-androideabi-4.8/prebuilt/linux-x86/bin/下。
###调试命令addr2line
addr2line 0x08048258 -e test -f
##gdbserver
在android端运行gdbserver :1234 --attach 96   #:1234是端口号，96 是进程ID    
在pc端运行adb forward tcp:1234 tcp:1234  #端口映射，将pc机的1235端口映射到手机的1234端口        
运行arm-linux-androideabi-gdb   

    (gdb) target remote:1234

