---
layout: post
title: "环形buffer"
date: 2014-08-04 16:00:14 +0000
comments: true
categories: linux
---

[示例代码](https://github.com/zhangshy/myTest/blob/master/ringBuffer/ringBuffer.c)      
###定义
    #define BUFFER_SIZE 0xF 
    typedef struct {
        uint32_t wptr;   ///< buffer存储位置，索引范围：[0:2*BUFFER_SIZE+1]
        uint32_t rptr;   ///< buffer读取位置，索引范围：[0:2*BUFFER_SIZE+1]
        uint8_t buffer[BUFFER_SIZE+1];  ///< buffer大小为BUFFER_SIZE+1
    }ring_buffer_t; 
###使用
若BUFFER_SIZE=3，wptr和rptr的索引为[0:7]    
1. wptr=rptr时buffer为空        
2. wptr和rptr之间的差值为BUFFER_SIZE+1即4时buffer为满       
3. wptr和rptr的范围通过pbuf->wptr &= (BUFFER_SIZE<<1)|0x1 确定，BUFFER_SIZE的取值应该是N个2进制1？？     

    __inline uint32_t ring_buffer_is_empty(ring_buffer_t *pbuf) {
        return (pbuf->wptr==pbuf->rptr);
    }

    __inline uint32_t ring_buffer_is_full(ring_buffer_t *pbuf) {
        return ((pbuf->wptr^pbuf->rptr)==(BUFFER_SIZE+1));
    }

    __inline void ring_buffer_flush(ring_buffer_t *pbuf) {
        pbuf->wptr = pbuf->rptr = 0;
    }

    void fill_ring_buffer(ring_buffer_t *pbuf, uint8_t data) {
        pbuf->buffer[pbuf->wptr&BUFFER_SIZE] = data;
        pbuf->wptr++;
        pbuf->wptr &= (BUFFER_SIZE<<1)|0x1;
    }

    uint8_t dump_ring_buffer(ring_buffer_t *pbuf) {
        uint8_t data;
        data = pbuf->buffer[pbuf->rptr&BUFFER_SIZE];
        pbuf->rptr++;
        pbuf->rptr &= (BUFFER_SIZE<<1)|0x1;

        return data;
    }
