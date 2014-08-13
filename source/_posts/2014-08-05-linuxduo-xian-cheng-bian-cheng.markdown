---
layout: post
title: "linux多线程编程"
date: 2014-08-05 17:28:29 +0000
comments: true
categories: linux
---

[linux多线程编程](http://www.cnblogs.com/BiffoLee/archive/2011/11/18/2254540.html)      
###pthread_create
    void *test_thread(void *arg) {
        int *num = (int *)arg;  //可以得到参数
    }

    pthread_t thread_id;
    int testNum=1;
    pthread_create(&thread_id, NULL, test_thread, (void *)&testNum);
###pthread_detach与pthread_join
调用pthread_join(pthread_id)后，如果该线程没有运行结束，调用者会被阻塞;这时可以在子线程中加入代码      
pthread_detach(pthread_self())      
或者父线程调用      
pthread_detach(thread_id)（非阻塞，可立即返回）     
这将该子线程的状态设置为detached,则该线程运行结束后会自动释放所有资源

###pthread_cond_wait
[pthread_cond_wait](http://baike.baidu.com/view/5725833.htm?fr=aladdin)     
条件变量是利用线程间共享的全局变量进行同步的一种机制，主要包括两个动作：一个线程等待"条件变量的条件成立"而挂起；另一个线程使"条件成立"（给出条件成立信号）。为了防止竞争，条件变量的使用总是和一个互斥锁结合在一起。        

    pthread_mutex_t mutex;
    pthread_cond_t cond;
    pthread_mutex_init(&mutex, NULL);
    pthread_cond_init(&cond, NULL);

    //等待
    pthread_mutex_lock(&mutex);
    pthread_cond_wait(&cond, &mutex);
    pthread_mutex_unlock(&mutex)
    //激活
    pthread_mutex_lock(&mutex);
    pthread_cond_signal(&cond);
    pthread_mutex_unlock(&mutex);
    //注销
    pthread_mutex_destroy(&mutex);
    pthread_cond_destroy(&cond);

