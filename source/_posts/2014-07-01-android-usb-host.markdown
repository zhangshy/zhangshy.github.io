---
layout: post
title: "android usb host"
date: 2014-07-01 14:18:15 +0000
comments: true
categories: android
---

###1. 检查系统是否开启了usb host功能
      确认/system/etc/permissions目录有android.hardware.usb.host.xml这个文件。

###2. 查找设备
    在AndroidManifest.xml中添加：
    <intent-filter>
        <action android:name="android.hardware.usb.action.USB_DEVICE_ATTACHED" />
    </intent-filter>
    <meta-data android:name="android.hardware.usb.action.USB_DEVICE_ATTACHED"
    android:resource="@xml/device_filter" />

res/xml/device_filter.xml的内容如下，注意为10进制数

    <?xml version="1.0" encoding="utf-8"?>
    <resources>
        <usb-device vendor-id="1234" product-id="5678" class="255" subclass="66" protocol="1" />
    </resources>

####方法1
    在<activity ...></activity>之间加intent-filter之间，这样当USB设备插入时会启动这个Activity，通过
    UsbDevice device = (UsbDevice) intent.getParcelableExtra(UsbManager.EXTRA_DEVICE);获取设备

####方法2
不用系统的广播的话，可以遍历设备列表，得到设备

    UsbManager manager = (UsbManager) getSystemService(Context.USB_SERVICE);
    ...
    HashMap<String, UsbDevice> deviceList = manager.getDeviceList();
    Iterator<UsbDevice> deviceIterator = deviceList.values().iterator();
    while(deviceIterator.hasNext()){
        UsbDevice device = deviceIterator.next()
        //查找想要的设备
        if (device.getVenderId()==venderID && device.getProductId==productId)
            //your code
    }

通过UsbManager的requestPermission方法申请usb权限
manager.requestPermission(device, null);会弹出选择框让用户选择是否允许应用访问usb设备。
###3. 通信
官网例子

    private Byte[] bytes
    private static int TIMEOUT = 0;
    private boolean forceClaim = true;
    ...
    UsbInterface intf = device.getInterface(0);
    UsbEndpoint endpoint = intf.getEndpoint(0);
    UsbDeviceConnection connection = mUsbManager.openDevice(device); 
    connection.claimInterface(intf, forceClaim);
    connection.bulkTransfer(endpoint, bytes, bytes.length, TIMEOUT); //do in another thread
    
获取usb设备端点

      UsbEndpoint ep = intf.getEndpoint(i);
      ep.getType() == UsbConstants.USB_ENDPOINT_XFER_INT;//中断
      ep.getDirection()==UsbConstants.USB_DIR_OUT;//相对主机来说
      
####使用UsbRequest
发送

    ByteBuffer mOutData = ByteBuffer.allocate(len);
    UsbRequest mUsbRequest = new UsbRequest();
    mUsbRequest.initialize(connection, mEndpointOut);
    mUsbRequest.queue(mOutData, len);
    if (connection.requestWait()==mUsbRequest)
        //成功
        
接收，不知道为什么在4.2上mInData要ByteBuffer.allocate(len*2)
    
    //ByteBuffer mInData = ByteBuffer.allocate(len);//4.0
    ByteBuffer mInData = ByteBuffer.allocate(len*2);
    UsbRequest mUsbRequest = new UsbRequest();
    mUsbRequest.initialize(connection, mEndpointIn);
    mUsbRequest.queue(mInData, len);
    if (connection.requestWait()==mUsbRequest)
        byte[] data = mInData.array();
