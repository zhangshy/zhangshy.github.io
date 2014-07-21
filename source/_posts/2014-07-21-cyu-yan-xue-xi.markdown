---
layout: post
title: "c语言学习"
date: 2014-07-21 10:30:30 +0000
comments: true
categories: c
---

###VA_ARGS
可变参数宏，如果可变参数被忽略或为空, ‘##’操作将使预处理器(preprocessor)去除掉它 前面的那个逗号

    #define dePrintf(X,...) printf("[%s %s: %d]" X, __FILE__, __func__, __LINE__, ##__VA_ARGS__)
###cpp引用c函数库
    #ifdef __cplusplus
    extern "C"{
    #endif
    /* c++引用C，将#include "x264.h"放到extern "C" 中 */
    #include "x264.h"
    #ifdef __cplusplus
    }
    #endif
    
    

