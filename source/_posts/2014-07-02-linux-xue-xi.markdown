---
layout: post
title: "linux 学习"
date: 2014-07-02 23:00:20 +0000
comments: true
categories: linux
---

####vim
:vsplit 将窗口分成两个，ctrl+w切换  
:edit 打开文件  
:%!xxd -g 1 ：切换成十六进制的一个字节的模式    
u 撤销上一步的操作  
ctrl+r 恢复上一步被撤销的操作   
shift+8 查找当前单词    
ggVG 全选   
q: 进入命令历史编辑     
vim xxx +N  打开文件并跳到第N行    
vim -d xx xxx 等同于 vimdiff xx xxx     
:set noro   取消只读，在git difftool时很有用    
dp将当前光标内容复制到另一窗口，do将另一窗口内容复制过来    

####tar
查看内容不解压：tar tvf xxx.tar.gz  
解压：tar -xzvf xxx.tar.gz  
解压到指定目录：tar -xzvf xxx.tar.gz -C /XX/XXX     
压缩：tar -czvf xxx.tar.gz xxx/     
解压：tar -xjvf xxx.tar.bz2 

####mount
mount后让普通用户有读写权限，加参数-o umask=000

    sudo mount /dev/sdb1 /mnt/ -o umask=000

挂载linux网络设备

    mount -t nfs -o nolock 192.168.0.188:/share /mnt
####pkg-config
    查看头文件：pkg-config --cflags opencv
    查找库文件：pkg-config --libs opencv
    查看版本： pkg-config --modversion opencv
####cp
连带目录一起复制： cp --parents -r xx/xx/xx/ xx/
###sed
'-i'使修改生效  
a. 将old替换为new： sed 's/old/new/g' file -i  
b. 删除第四行： sed '4 d' file  
c. 在第三行后添加一行： sed '3 a xxx' file  
d. 在第三行前添加一行： sed '3 i xxx' file
###find和grep
在.c文件中查找东西：find -name '*.c' | xargs grep 'xxx'
###gcc编译时指定小端-EL
mips-linux-gnu-gcc -g -O2 -Wall -EL test.c

###watch
检测一个命令的运行结果，默认2S间隔重复运行命令，用-n参数指定时间间隔: watch
free
