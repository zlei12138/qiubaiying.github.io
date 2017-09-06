---
layout:     post
title:      BroadcastReceiver常见问题
subtitle:   开发，面试可能遇到
date:       2016-12-08
author:     ZL
header-img: img/BroadcastReceiver-point.jpg
catalog: 	 true
tags:
    - BroadcastReceiver
    - Android
---

## 关于BroadcastReceiver ##
BroadcastReceiver 自身并不实现图形用户界面，但是当它收到某个通知后， BroadcastReceiver 可以通过启动 Service 、启动 Activity 进行操作或是 NotificationMananger 提醒用户。

### 注册 ###
#### 静态方式（AndroidManifest.xml里面） ####
```xml
< receiver android:name = ".MyBroadcastReceiver" >
 < intent-filter android:priority = "777" > //-1000-1000
<action android:name = "android.provider.Telephony.SMS_RECEIVED" />
</ intent-filter >
</ receiver >
```
#### 动态方式（代码里面） ####
```java
public class MainActivity extends Activity {
    MyBroadcastReceiver receiver;
    [@Override](/user/Override)
     protected void onResume() {
        // 动态注册广播 (代码执行到这才会开始监听广播消息，并对广播消息作为相应的处理)
        receiver = new MyBroadcastReceiver();
        IntentFilter intentFilter = new IntentFilter( "android.provider.Telephony.SMS_RECEIVED" );
        registerReceiver( receiver , intentFilter);

        super.onResume();
    }
    [@Override](/user/Override)
    protected void onPause() {
        // 撤销注册 (撤销注册后广播接收者将不会再监听系统的广播消息)
        unregisterReceiver(receiver);
        super.onPause();
    }
}
```
#### 动态与静态的区别 ####
1. 静态注册的广播接收者一经安装就常驻在系统之中，不需要重新启动唤醒接收者； 动态注册的广播接收者随着应用的生命周期，由registerReceiver开始监听，由unregisterReceiver撤销监听，如果应用退出后，没有撤销已经注册的接收者应用应用将会报错。

2. 当广播接收者通过intent启动一个activity或者service时，如果intent中无法匹配到相应的组件。动态注册的广播接收者将会导致应用报错而静态注册的广播接收者将不会有任何报错，因为自从应用安装完成后，广播接收者跟应用已经脱离了关系。　

### 接收者 ###
```java
public class MyBroadcastReceiver extends BroadcastReceiver {

// action 名称
String SMS_RECEIVED = "android.provider.Telephony.SMS_RECEIVED" ;

    public void onReceive(Context context, Intent intent) {

       if (intent.getAction().equals( SMS_RECEIVED )) {
           // 一个receiver可以接收多个action的，即可以有多个intent-filter，需要在onReceive里面对intent.getAction(action name)进行判断。
       }
    }
}
```
### 发送广播的类型 ###
#### 普通广播 ####
所有接收者接收到，所有满足条件的 BroadcastReceiver 都会随机地执行其 onReceive() 方法。无法中断。
```java
Intent intent = new Intent("android.provider.Telephony.SMS_RECEIVED");
//通过intent传递少量数据
intent.putExtra("data", "finch");
// 发送普通广播
sendBroadcast(Intent);
```
#### 有序广播 ####

```java
//发送有序广播
 sendOrderedBroadcast(intent, null);
 ```
 在有序广播中，我们可以在前一个广播接收者将处理好的数据传送给后面的广播接收者，也可以调用abortBroadcast()来终结广播的传播。高级别的广播收到该广播后，可以决定把该广播是否截断掉.
 ```java
 public void onReceive(Context arg0, Intent intent) {
　　//获取上一个广播的bundle数据
　　Bundle bundle = getResultExtras(true);//true：前一个广播没有结果时创建新的Bundle；false：不创建Bundle
　　bundle.putString("key", "777");
　　//将bundle数据放入广播中传给下一个广播接收者
　　setResultExtras(bundle);　
　　
　　//终止广播传给下一个广播接收者
　　abortBroadcast();
}
```
### 总结 ###
- 当系统或应用发出广播时，将会扫描系统中的所有广播接收者，通过action匹配将广播发送给相应的接收者，接收者收到广播后将会产生一个广播接收者的实例，执行其中的onReceiver()这个方法；特别需要注意的是这个实例的生命周期只有10秒，如果10秒内没执行结束onReceiver()，系统将会报错。在onReceiver()执行完毕之后，该实例将会被销毁，所以不要在onReceiver()中执行耗时操作，也不要在里面创建子线程处理业务（因为可能子线程没处理完，接收者就被回收了，那么子线程也会跟着被回收掉）；正确的处理方法就是通过in调用activity或者service处理业务。
- 静态广播接收的处理器是由PackageManagerService负责，当手机启动或者新安装了应用的时候，PackageManagerService会扫描手机中所有已安装的APP应用，将AndroidManifest.xml中有关注册广播的信息解析出来，存储至一个全局静态变量当中。
- 动态广播接收的处理器是由ActivityManagerService负责，当APP的服务或者进程起来之后，执行了注册广播接收的代码逻辑，即进行加载，最后会存储在一个另外的全局静态变量中。需要注意的是：
