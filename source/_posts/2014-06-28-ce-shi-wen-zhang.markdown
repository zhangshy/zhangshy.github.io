---
layout: post
title: "测试文章"
date: 2014-06-28 15:57:06 +0000
comments: true
categories: 
---

![myself](http://hd42.xiaonei.com/photos/hd42/20080422/19/21/large_3444b150.jpg)
##archlinux安装
###pacman
    pacman -Sy abc                    和源同步后安装名为abc的包
    pacman -S abc                     从本地数据库中得到abc的信息，下载安装abc包
    pacman -Sf abc                    强制安装包abc
    pacman -Ss abc                   搜索有关abc信息的包
    pacman -Si abc                    从数据库中搜索包abc的信息
    pacman -Syu                        同步源，并更新系统
    pacman -Sy                          仅同步源
    pacman -R abc                     删除abc包
    pacman -Rc abc                   删除abc包和依赖abc的包
    pacman -Rsn abc                 移除包所有不需要的依赖包并删除其配置文件
    pacman -Sc                          清理/var/cache/pacman/pkg目录下的旧包
    pacman -Scc                        清除所有下载的包和数据库
    pacman -Sd abc                   忽略依赖性问题，安装包abc
    pacman -Su --ignore foo       升级时不升级包foo
    pacman -Sg abc                   查询abc这个包组包含的软件包
    pacman -Q                           列出系统中所有的包
    pacman -Q package             在本地包数据库搜索(查询)指定软件包
    pacman -Qi package            在本地包数据库搜索(查询)指定软件包并列出相关信息
    pacman -Q | wc -l                  统计当前系统中的包数量
    pacman -Qdt                         找出孤立包
    pacman -Rs $(pacman -Qtdq) 删除孤立软件包（递归的,小心用)
    pacman -U   abc.pkg.tar.gz      安装下载的abs包，或新编译的本地abc包
    pacman-optimize && sync        提高数据库访问速度
###输入法fcitx
      按照例子安装fcitx后，还要安装pacman -S fcitx-googlepinyin。  
      配置输入法，在Input Method -> 去掉Only Show Currentt Language勾 -> 搜索Chinese 选择

###声音
    # pacman -S alsa-utils
    # alsamixer
    注意要[M] 取消静音

###pyuv的deb安装
archlinux安装deb文件，将deb文件解压后复制

    //先解压
    ar vx pyuv_0.5-1_x86_64.deb
    tar -xzvf data.tar.gz 
    //复制
    cp usr/bin/pyuv /usr/bin/

运行pyuv出错，pyuv: error while loading shared libraries: libwx_gtk2u_richtext-2.8.so.0: cannot open shared object file: No such file or directory      
安装依赖库：pacman -S wxgtk2.8
###安装adb
64为系统安装32位程序需要安装库，对/etc/pacman.conf做如下修改

    -#[multilib]
    -#Include = /etc/pacman.d/mirrorlist
    +[multilib]
    +Include = /etc/pacman.d/mirrorlist

运行pacman -Sy同步包数据库，安装pacman -S lib32-glibc和pacman -S lib32-libstdc++5; pacman -S lib32-ncurses ：lib32-ncurses也安装了，不知是否必须。

###串口工具
安装minicom: pacman -S minicom  
打开usb转串口：minicom -D /dev/ttyUSB0  
设置界面：ctrl+a 后点击o，打开设置界面; ctrl+a后点击l设置保存文件;
ctrl+a后z帮助界面

###github博客octopress
[在Github上搭建Octopress博客](http://www.ectogo.com/blog/2014/03/25/settingupoctopressblogongithub/)        
####新电脑 老博客
    git clone git@github.com:username/username.github.io.git
    cd username.github.io
    git checkout source
    mkdir _deploy
    cd _deploy
    git init
    git remote add origin git@github.com:username/username.github.io.git
    git pull origin master
    cd ..

下次切换电脑的时候只需要更新一下

    #先更新source
    git pull origin source
    #更新master
    cd _deploy
    git pull origin master
    cd ..
