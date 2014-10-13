---
layout: post
title: "linux测试常用命令"
date: 2014-09-30 12:50:52 +0000
comments: true
categories: linux
---

###关内核打印
内核打印在串口上输出时有时会影响系统响应速度，尤其是内核中断里的打印，如果中断中打印在串口上输出，有时会造成意想不到的错误  
    echo 0 > /proc/sys/kernel/printk    
所有的内核打印都不在串口上输出，默认可使用4。       
查看内核打印有两个方法dmesg或cat /proc/kmsg
###查看cpu主频
cat /sys/devices/system/cpu/cpu0/cpufreq/scaling_cur_freq
###查看进程内存使用
cat /proc/[pid]/status其中VmRSS一项就是内存使用情况

