---
layout:     post
title:      Service和IntentService常见问题
subtitle:   面试和开发常见问题
date:       2017-09-03
author:     ZL
header-img: img/blog_service.jpg
catalog: 	 true
tags:
    - Service
    - IntentService
    - Android
---

# 关于Service #
服务是四大组件之一，可以长期运行在后台，没有用户操作的界面，用作播放音乐或者网络传输等等，并且当app被切换到后台以后，其依然可以运行。

## 与Thread的区别 ##
二者并没有关系，thread是一个线程，service是一个组件，服务的默认也是运行于主线程中的，如果是耗时操作，依然需要在服务中创建线程。

## 服务的分类 ##

### 按运行分类 ###

1. 前台服务：不会被杀死，而且会同时要求设置一个通知栏，用户会知道这个服务的存在。

    使用代码：
    ```java
    Notification notification = new Notification(R.drawable.icon, getText(R.string.ticker_text),System.currentTimeMillis());

    Intent notificationIntent = new Intent(this,ExampleActivity.class);

    PendingIntent pendingIntent = PendingIntent.getActivity(this, 0, notificationIntent, 0);

    notification.setLatestEventInfo(this, getText(R.string.notification_title),
            getText(R.string.notification_message), pendingIntent);

    startForeground(ONGOING_NOTIFICATION, notification);

    //取消前台服务
    stopForeground(true)
    //这个方法只是说让服务不是前台了，但是并没有终止此服务。true和false表示是否同时移除状态栏。
    ```


2. 后台服务：正常的服务。

### 按使用分类 ###

1. 本地服务：用于应用程序内部，通过startService和stopService开启和结束，也可以自己结束，通过service.stopSelf()或者service.stopSelfResult()结束自己。

2. 远程服务：除了被自己调用还可以被其他的应用程序调用，通过context.bindService 和context.unbindService 绑定和解绑。一个服务可以被多个客户端绑定。

## Service 的生命周期 ##
分两种：

![service生命周期](http://ovoxjpcrm.bkt.clouddn.com/25ed3985286db9af31a3650c9d94636d.png)

首先解释一下上图的那些方法：
> - onCreate:服务正在被创建调用。
>
> - onStartCommand:服务正在启动，由startService触发。返回一个int值，决定服务被杀死后的处理方式。可能重建也可能不重建服务。
>
> - onBind:bindService触发。
>
> - onUnbind:unbindService触发。
>
> - onRebind：onUnbind以后又bindService。
>
> - onDestory:服务被销毁。

## 两种方式开服务的区别与结合 ##
start服务：**被创建以后就与调用者无关，调用者死了，它也并不会死。调用者是无法使用服务里面的方法的。** 如果服务被startService很多次，onCreate只会走一次，但是onStart会走很多次，可是Service的实例就只有一个。等到调用者stopService或者服务自己stopSelf,销毁服务。

bind服务：**可以被多个调用者绑定，调用者全部销毁后，服务也会被销毁。调用者是可以使用其中的方法的。** 如果服务被某个activity bindService 很多次，onCreate只会走一次，onStart不会走。

bindService的代码复杂一些，代码如下：
```java
//调用者
public class MainActivity extends Activity {
    /** 是否绑定 */
    boolean mIsBound = false;
    BindService mBoundService;
    [@Override](/user/Override)
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        doBindService();
    }
    /**
     * 实例化ServiceConnection接口的实现类,用于监听服务的状态
     */
    private ServiceConnection conn = new ServiceConnection() {

        [@Override](/user/Override)
        public void onServiceConnected(ComponentName name, IBinder service) {
            BindService mBoundService = ((BindService.MyBinder) service).getService();

        }

        [@Override](/user/Override)
        public void onServiceDisconnected(ComponentName name) {
            mBoundService = null;

        }
    };

    /** 绑定服务 */
    public void doBindService() {
        bindService(new Intent(MainActivity.this, BindService.class), conn,Context.BIND_AUTO_CREATE);
        mIsBound = true;
    }

    /** 解除绑定服务 */
    public void doUnbindService() {
        if (mIsBound) {
            // Detach our existing connection.
            unbindService(conn);
            mIsBound = false;
        }
    }

    [@Override](/user/Override)
    protected void onDestroy() {
        // TODO Auto-generated method stub
        super.onDestroy();

        doUnbindService();
    }
}
  }


  //服务类
  public class BindService extends Service {

       // 实例化MyBinder得到mybinder对象；
      private final MyBinder binder = new MyBinder();

        /**
       * 返回Binder对象。
       */
      [@Override](/user/Override)
      public IBinder onBind(Intent intent) {
          // TODO Auto-generated method stub
          return binder;
      }

       /**
        * 新建内部类MyBinder，继承自Binder(Binder实现IBinder接口),
        * MyBinder提供方法返回BindService实例。
        */
  　　public class MyBinder extends Binder{

          public BindService getService(){
              return BindService.this;
          }
      }


      [@Override](/user/Override)
      public boolean onUnbind(Intent intent) {
          // TODO Auto-generated method stub
          return super.onUnbind(intent);
      }
  }
```

需求：既要求Service不随着Activity的销毁而销毁，又想要调用Service里面的方法。
解决：混合开启（顺序很重要）
1. startService
2. bindService
3. 调用服务方法
4. unbindService
5. stopService


## 其他 ##

- 如果bindService以后屏幕旋转了，那么activity被销毁了，之前与service的绑定也会断开。

- 使用Service需要在manifest里面声明

- 判断Service是否在运行：
```java
private boolean isServiceRunning() {
    ActivityManager manager = (ActivityManager) getSystemService(ACTIVITY_SERVICE);
    {
        if ("com.example.demo.BindService".equals(service.service.getClassName())) 　　{
            return true;
        }
    }
    return false;
}
```

# 关于IntentService
代码：
```java
//服务类
public class myIntentService extends IntentService {

    //------------------必须实现-----------------------------

    public myIntentService() {
        super("myIntentService");
        // 注意构造函数参数为空，这个字符串就是worker thread的名字
    }

    [@Override](/user/Override)
    protected void onHandleIntent(Intent intent) {
        //根据Intent的不同进行不同的事务处理
        String taskName = intent.getExtras().getString("taskName");
        switch (taskName) {
        case "task1":
            Log.i("myIntentService", "do task1");
            break;
        case "task2":
            Log.i("myIntentService", "do task2");
            break;
        default:
            break;
        }
    }
  //--------------------用于打印生命周期--------------------
   [@Override](/user/Override)
  public void onCreate() {
        Log.i("myIntentService", "onCreate");
    super.onCreate();
}

    [@Override](/user/Override)
    public int onStartCommand(Intent intent, int flags, int startId) {
        Log.i("myIntentService", "onStartCommand");
        return super.onStartCommand(intent, flags, startId);
    }

    [@Override](/user/Override)
    public void onDestroy() {
        Log.i("myIntentService", "onDestroy");
        super.onDestroy();
    }
}

//调用者
public class MainActivity extends Activity {
    [@Override](/user/Override)
    protected void onCreate(Bundle savedInstanceState) {
        // TODO Auto-generated method stub
        super.onCreate(savedInstanceState);

        //同一服务只会开启一个worker thread，在onHandleIntent函数里依次处理intent请求。

        Intent i = new Intent("cn.scu.finch");
        Bundle bundle = new Bundle();
        bundle.putString("taskName", "task1");
        i.putExtras(bundle);
        startService(i);

        Intent i2 = new Intent("cn.scu.finch");
        Bundle bundle2 = new Bundle();
        bundle2.putString("taskName", "task2");
        i2.putExtras(bundle2);
        startService(i2);

        startService(i);  //多次启动
    }
}
```

讲解：
IntentService 就是Service 的加强版，平常如果在Service里面执行耗时操作，需要自己手动的去在异步线程中处理，IntentService 直接给我们处理好了，我们只要处理onHandleIntent方法就好了，减少工作量。

注意：
构造方法一定要按照代码给的那样写
只有一个线程，诶个去处理intent请求，当所有intent请求处理完以后，会自动关闭服务。不用调用stopSelf.

上面的代码的结果：

![IntentService运行结果](http://ovoxjpcrm.bkt.clouddn.com/28985b709878e3f67cedc125d960de9f.png)

它只有一个工作线程，名字就是构造函数的那个字符串，也就是“myIntentService”，我们知道多次开启service，只会调用一次onCreate方法（创建一个工作线程），多次onStartCommand方法（用于传入intent通过工作队列再发给onHandleIntent函数做处理）。
