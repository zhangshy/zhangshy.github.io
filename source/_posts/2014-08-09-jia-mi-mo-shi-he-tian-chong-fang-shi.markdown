---
layout: post
title: "加密模式和填充方式"
date: 2014-08-09 14:30:00 +0000
comments: true
categories: 
---

###模式介绍
1. 电码本模式(Electronic Codebook Book(ECB))    
这种模式是将整个明文分成若干段相同的小段，然后对每一小段进行加密。  
2. 密码分组链接模式(Cipher Block Chaining (CBC))    
这种模式是先将明文切分成若干小段，然后每一小段与初始块或者上一段的密文段进行异或运算后，再与密钥进行加密。
###填充方式
[JCE中支持AES，支持的模式和填充方式](http://blog.chinaunix.net/uid-196845-id-2788287.html)      
JCE中AES支持五中模式：CBC，CFB，ECB，OFB，PCBC；支持三种填充：NoPadding，PKCS5Padding，ISO10126Padding。不支持SSL3Padding。不支持“NONE”模式。
 
其中AES/ECB/NoPadding和我现在使用的AESUtil得出的结果相同(在16的整数倍情况下)。不带模式和填充来获取AES算法的时候，其默认使用ECB/PKCS5Padding。
 
    算法/模式/填充                   16字节加密后数据长度         不满16字节加密后长度
    AES/CBC/NoPadding              16                         不支持
    AES/CBC/PKCS5Padding           32                         16
    AES/CBC/ISO10126Padding        32                         16
    AES/CFB/NoPadding              16                         原始数据长度
    AES/CFB/PKCS5Padding           32                         16
    AES/CFB/ISO10126Padding        32                         16
    AES/ECB/NoPadding              16                         不支持
    AES/ECB/PKCS5Padding           32                         16
    AES/ECB/ISO10126Padding        32                         16
    AES/OFB/NoPadding              16                         原始数据长度
    AES/OFB/PKCS5Padding           32                         16
    AES/OFB/ISO10126Padding        32                         16
    AES/PCBC/NoPadding             16                         不支持
    AES/PCBC/PKCS5Padding          32                         16
    AES/PCBC/ISO10126Padding       32                         16
 
可以看到，在原始数据长度为16的整数倍时，假如原始数据长度等于16n，则使用NoPadding时加密后数据长度等于16n，其它情况下加密数据长度等于16(n+1)。在不足16的整数倍的情况下，假如原始数据长度等于16n+m[其中m小于16]，除了NoPadding填充之外的任何方式，加密数据长度都等于16(n+1)；NoPadding填充情况下，CBC、ECB和PCBC三种模式是不支持的，CFB、OFB两种模式下则加密数据长度等于原始数据长度。
