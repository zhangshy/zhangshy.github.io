---
layout: post
title: "linux驱动"
date: 2014-07-09 16:08:06 +0000
comments: true
categories: linux
---

###创建设备
class_create和device_create函数，在/dev下创建设备节点

    int my_device_major = register_chrdev(0, "myDevice", &my_device_fops);
    struct class* my_device_class = class_create(THIS_MODULE, "myDevice");
    //在/sys/class下创建了myDevice目录
    if (IS_ERR(my_device_class))
        return -1;
    device_create(my_device_class, NULL, MKDEV(my_device_major, 0), NULL, "myDevice");
    //在/dev下创建myDevice设备
###自旋锁spinlock_t
spin_lock_irqsave会关闭中断

    spinlock_t rx_slock;
    spin_lock_init(&rx_slock);  //初始化
    /* 使用 */
    unsigned long flags;
    spin_lock_irqsave(&rx_slock, flags);
    ...
    //处理
    ...
    spin_unlock_irqrestpre(&rx_slock, flags);

###poll
####驱动poll_wait
    wait_queue_head_t rd_wq;
    init_waitqueue_head(&rd_wq);

    unsigned int test_poll(struct file *filp, struct poll_table_struct *wait){
        unsigned int ret = 0;
        poll_wait(filp, &rd_wq, wait);
        ...//处理
        if (hasData) ret |= POLLIN|POLLRDNORM;
        return ret;
    }

    当收到数后：wake_up_interruptible(&rd_wq);

####应用poll
    int fd = open("/dev/xxx", O_RDWR);
    uint32_t timeout = xxxxx;
    struct pollfd pfd;
    pfd.fd = fd;
    pfd.events = POLLIN;
    ret = poll(&pfd, 1, timeout);
    if (ret!=1)
        return -1;
    ret = read(fd, (void*)data, *size);




