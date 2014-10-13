---
layout: post
title: "NDK编译zlib和libpng"
date: 2014-09-26 16:01:23 +0000
comments: true
categories: android
---

下载zlib-1.2.8.tar.xz和libpng-1.6.13.tar.xz，解压tar -xJvf xx.tar.xz
###编译zlib
编写zlibbuild.mk文件，对照源码下的win32/Makefile.gcc文件填写LOCAL_SRC_FILES项：
    
    LOCAL_PATH:= $(call my-dir)  
  
    include $(CLEAR_VARS)  
        
    LOCAL_SRC_FILES := \
        adler32.c \
    	compress.c \
    	crc32.c \
    	deflate.c \
    	gzclose.c \
    	gzlib.c \
    	gzread.c \
    	gzwrite.c \
    	infback.c \
    	inffast.c \
    	inflate.c \
    	inftrees.c \
    	trees.c \
    	uncompr.c \
    	zutil.c  
      
    LOCAL_MODULE:= libz  
      
    include $(BUILD_STATIC_LIBRARY)
    
编写build文件   

    ndk-build \
    NDK_PROJECT_PATH=`pwd` \
	APP_ABI=armeabi-v7a \
	APP_PLATFORM=android-14 \
	APP_BUILD_SCRIPT=`pwd`/zlibbuild.mk \
	TARGET_PRODUCT=alltarget \
	$@
    
执行. build编译

###编译libpng
参照源码下的scripts/makefile.gcc文件填写LOCAL_SRC_FILES项：

    LOCAL_PATH := $(call my-dir)
    include $(CLEAR_VARS)
    
    LOCAL_SRC_FILES := \
        png.c \
    	pngerror.c \
    	pngget.c \
    	pngmem.c \
    	pngpread.c \
    	pngread.c \
    	pngrio.c \
    	pngrtran.c \
    	pngrutil.c \
    	pngset.c \
    	pngtrans.c \
    	pngwio.c \
    	pngwrite.c \
    	pngwtran.c \
    	pngwutil.c
    
    LOCAL_MODULE := libpng
    
    LOCAL_LDLIBS := -lz
    
    include $(BUILD_SHARED_LIBRARY)
    
编译时提示缺少pnglibconf.h文件，解决办法使用源码下的scripts/pnglibconf.h.prebuilt文件，ln -s scripts/pnglibconf.h.prebuilt pnglibconf.h
