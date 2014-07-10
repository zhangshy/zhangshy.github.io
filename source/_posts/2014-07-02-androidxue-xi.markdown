---
layout: post
title: "android学习"
date: 2014-07-02 14:35:05 +0000
comments: true
categories: android
---

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

    private Hander mHandler = new Hander() {
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

