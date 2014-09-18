---
layout: post
title: "linux驱动"
date: 2014-07-09 16:08:06 +0000
comments: true
categories: linux
---

###交叉编译
编译ko文件

    Makefile:
    PWD=$(shell pwd)
    KDIR=/xx/xx/linux-3.4

    obj-m+=scr.o
    scr-objs := file1.o file2.o test-scr.o

    build:
        $(MAKE) -C $(KDIR) M=$(PWD)

    clean:
        @rm -rf *.o *.ko *.cmd *.mod.c *.order *.symvers *.tmp_versions *~

执行编译：

    make ARCH=arm CROSS_COMPILE=/xx/xx/arm-linux-gnueabi-
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

###workqueue工作队列在中断中使用
使用：在有大量的中断处理中，需要紧急处理的中断直接在中断处理函数中进行，可以延迟处理的中断放到了workqueue中做，（不知这样对不对，我看网上还有一种tasklet）。workqueue的使用：

    struct work_struct device_work;
    struct workqueue_struct *device_wq;

    void device_work_func(struct work_struct *work) {
        ...//处理中断的函数
    }
    //中断回调函数
    irqreturn_t device_irq_service(int irqno, void *dev_id) {
        unsigned int temp;
        unsinged long flags;
        //加锁
        spin_lock_irqsave(&rx_slock, flags);
        temp = my_get_device_interrupt_status();//一般都可以从寄存器中读到中断状态
        if (temp&MY_DEVICE_IMMEDIATE) { //需要立即处理的紧急中断
            ...//中断处理
        } else {
            queue_work(device_wq, &device_work);
        }
        spin_unlock_irqrestpre(&rx_slock, flags);
    }

    //初始化
    device_wq = create_singlethread_workqueue("device_wq");//只创建一个内核线程
    INIT_WORK(&device_work, device_work_func);
    request_irq(my_irq_no, device_irq_service, 0, "my_device", NULL);//注册中断服务
    
补充：
1. 中断处理中尽量不要加打印; 
2. 在硬中断处理函数device_irq_service中不能使用mutex_lock，如果必须加锁可使用自旋锁spinlock_t; 在软中断
函数device_work_func中可以加mutex_lock

