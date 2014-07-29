---
layout: post
title: "android学习"
date: 2014-07-02 14:35:05 +0000
comments: true
categories: android
---

##包属性
###sharedUserId
使用sharedUserId属性时取值需要包含点（dot），如android:sharedUserId="test"安装时会提示：Failure [INSTALL_PARSE_FAILED_BAD_SHARED_USER_ID]。使用android:sharedUserId="zsy.test"就可以了。
##Handler和HandlerThread
Handler会关联一个单独的线程和消息队列。Handler默认关联主线程，会在主线程执行，如果在这里面的操作会有阻塞，界面也会卡住。如果要在其他线程执行，可以使用HandlerThread

    HandlerThread mHandlerThread = new HandlerThread("test");
    mHandlerThread.start();
    Handler mHandler = new Handler(mHandlerThread.getLooper()) {
        public void handlerMessage(Message msg) {
            XXX
        }
    };

当要停止mHandlerThread时可执行：

    mHandlerThread.quit();
##Thread和Runnable
[Java中Runnable和Thread的区别](http://developer.51cto.com/art/201203/321042.htm)

在java中可有两种方式实现多线程，一种是继承Thread类，一种是实现Runnable接口；Thread类是在java.lang包中定义的。一个类只要继承了Thread类同时覆写了本类中的run()方法就可以实现多线程操作了，但是一个类只能继承一个父类，这是此方法的局限

在程序开发中只要是多线程肯定永远以实现Runnable接口为主，因为实现Runnable接口相比继承Thread类有如下好处：1.避免点继承的局限，一个类可以继承多个接口;2.适合于资源的共享      

    package org.demo.runnable;  
    class MyThread implements Runnable{  
        private int ticket=10;  
        public void run(){  
            for(int i=0;i<20;i++){  
                if(this.ticket>0){  
                    System.out.println("卖票：ticket"+this.ticket--);  
                }  
            }  
        }  
    }  
    package org.demo.runnable;  
    public class RunnableTicket {  
        public static void main(String[] args) {  
            MyThread mt=new MyThread();  
            new Thread(mt).start();//同一个mt，但是在Thread中就不可以，如果用同一  
            new Thread(mt).start();//个实例化对象mt，就会出现异常  
            new Thread(mt).start();  
        }  
    }; 
##Button
使用Button的时候可以这么用：

    public class MainActivity extends Activity implements View.OnClickListener {
        ...
        Button btn_ok = (Button) findViewById(R.id.btn_ok);
        btn_ok.setOnClickListener(this);
    }
    
实现onClick(View v)方法，而且费时的按键响应操作使用Handler做

    public void onClick(View v) {
        switch(v.getId()) {
        case R.id.btn_ok:
            Message m = new Message();
            m.what = XXXX;
            Bundle data = new Bundle();
            data.putString(XXX, XXXXX);
            m.setData(data);
            mHandler.sendMessage(m);
            break;
        }
    }

mHandler的写法

    private Handler mHandler = new Handler() {
        public void handleMessage(Message msg) {
            switch(msg.what) {
            case XXX:
                String str = msg.getData().getString(XXX);
                break;
            }
        }
    }

##像素
[px、dp和sp，这些单位有什么区别？](http://www.cnblogs.com/bjzhanghao/archive/2012/11/06/2757300.html)  
px指屏幕上物理像素点，不建议使用;
   因为100px的图片在不同手机上显示的实际大小可能不同
   
    You would use:    
        sp for font sizes
        dip for everything else.    
        dip==dp

##adb及常用调试指令
开启adb功能：start adbd     
查看设备：adb devices       
多于一个设备时指定设备：adb -s 192.168.11.84:5555 shell       
启动应用某界面：am start com.example.test/.MainActivity     
命令行安装应用：pm install xxx.apk      
命令行卸载应用：pm uninstall com.example.test       
##分析工具dumpsys
dumpsys用来给出android应用程序的信息：      

    dumpsys [Option]
            meminfo 内存信息
            cpuinfo cpu信息
            等很多选项
    service list可以列出服务
###mat使用
[安装配置](http://www.ibm.com/developerworks/cn/opensource/os-cn-ecl-ma/index.html)     
[How to enable Heap updates on my android client](http://stackoverflow.com/questions/3999087/how-to-enable-heap-updates-on-my-android-client)       

##签名文件
生成签名文件        

    keytool -genkey -alias sign.keystore -keyalg RSA -validity 20000 -keystore sign.keystore

签名        

    jarsigner -verbose -keystore sign.keystore -signedjar xxx_signed.apk xxx.apk sign.keystore

