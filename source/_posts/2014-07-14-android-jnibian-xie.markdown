---
layout: post
title: "android jni编写"
date: 2014-07-14 11:26:26 +0000
comments: true
categories: android
---

###java文件编写
    package com.example.jni

    public class Testjni {
        public native int jniRead(byte[] data, int len);
        public native int jniWrite(byte[] data, int len);
        static {
            System.loadLibrary("myJni");
        }
    }
###jni文件编写
jni文件的函数名要和java中的方法名对应起来，在jni中申请的空间要注意释放。

    #include <jni.h>
    #include <android/log.h>

    #define LOG_TAG "myJni.c"
    #define LOGI(...) __android_log_print(ANDROID_LOG_DEBUG, LOG_TAG, __VA__ARGS__)
    #define LOGE(...) __android_log_print(ANDROID_LOG_ERROR, LOG_TAG, __VA__ARGS__)

    jint Java_com_example_jni_jniRead(JNIEnv *env, jobject thiz,
        jbyteArray receiveBuf, jint slen) {
        uint8_t p_recv_buf[256] = {0};
        int len = slen;
        len = read(fd, p_recv_buf, slen);
        LOGI("rea len:%d\n", len);
        (*env)->setByteArrayRegion(env, receiveBuf, 0, len,
            (const jbyte*)p_recv_buf);
        return len;
    }

    jint Java_com_example_jni_jniWrite(JNIEnv *env, jobject thiz,
        jbyteArray sendBuf, jint len) {
        //申请空间
        jbyte *sendelems = (*env)->GetByteArrayElements(env, sendBuf, NULL);
        ...//处理函数
        //释放
        (*env)->ReleaseByteArrayElements(env, sendBuf, sendelems, 0);
        return 0;
    }

###Android.mk
    LOCAL_PATH:= $(call my-dir)
    include $(CLEAR_VARS)
    
    LOCAL_MODULE_TAGS := optional
    LOCAL_SRC_FILES:= \
        myJni.c
    LOCAL_LDLIBS := -llog

    LOCAL_SHARED_LIBRARIES += libam_adp liblog libcutils
    LOCAL_C_INCLUDES += ../inc
    #编译生成libmyJni.so
    LOCAL_MODULE := libmyJni
    #编译动态库
    include $(BUILD_SHARED_LIBRARY)

执行mm编译。（菜鸟级别解释：:=是赋值的意思，+=是追加的意思，$是引用某变量的值）补充说明：BUILD_EXECUTABLE表示以一个可执行程序的方式进行编译，include $(BUILD_PACKAGE)则是编译出一个apk，include $(BUILD_STATIC_JAVA_LIBRARY)则是编译出jar包。      
####LOCAL_MODULE_TAGS说明
LOCAL_MODULE_TAGS ：=user eng tests optional    
user: 指该模块只在user版本下才编译      
eng: 指该模块只在eng版本下才编译        
tests: 指该模块只在tests版本下才编译        
optional:指该模块在所有版本下都编译     
我一般都使用optional  

##我的例子
###javah生成h文件
在src目录下执行javah -jni com.example.jni.Getso后生成com_example_jni_Getso.h

