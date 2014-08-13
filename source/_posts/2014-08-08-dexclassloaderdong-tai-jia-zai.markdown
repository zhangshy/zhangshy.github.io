---
layout: post
title: "DexClassLoader动态加载"
date: 2014-08-08 11:40:07 +0000
comments: true
categories: android
---

[Android动态加载jar/dex](http://www.cnblogs.com/over140/archive/2011/11/23/2259367.html)        
[Android中的动态加载机制](http://blog.csdn.net/jiangwei0910410003/article/details/17679823)     
[示例代码](https://github.com/zhangshy/myTest/tree/master/DexClassLoaderTest)       
##1. 准备dex文件
###1.1 编写
接口：      

    package com.dex.test;

    public interface IDextest {
        public String getDexString();
    }
实现：      

    package com.dex.test;

    public class Dextest implements IDextest{
    	@Override
	    public String getDexString() {
		    return "here dex test!!";
	    }
    }
###1.2 导出jar包
![导出jar包](/images/cap_jar.jpg)       
导出时不用选择接口文件。
###1.3 使用dx转换
dx在sdk/build-tools/android-4.4.2/下，执行/opt/android/sdk/build-tools/android-4.4.2/dx --dex --output=test.jar dextest.jar

