
 title: 小议Android多进程以致Application多次初始化
 
 date: 2015-07-01 23:56:21
 
 tags: [Android，多进程] 

---

最近遇到一个bug，当应用加了多进程后，比如总共进程数为N，会出现在`startService（）`时`onStartCommand()`方法会被重复调用`（N-1）`次的奇怪现象。



***
## 祸起
>最近遇到两个模块互不相干却受到影响的奇怪问题，一个push模块和一个DaemonProcess模块在一起后，会出现如下现像的问题
***
当DaemonProcess为应用加了多进程后，比如总共进程数为N，会出现push模块在`startService（）`时`onStartCommand()`方法会被重复调用`（N-1）`次的奇怪现象。
***


<!--more-->



## 寻踪

*  因为我们用的是Jpush的原因，一开始以为是Jpush,但最后发现是因为引用多进程的原因
*  再寻找下去发现 调用一次`startService（）`时`onStartCommand()`运行多次
*  而这两者有何关系呢


## 举证

> Demo测试：
> 首先在Application中申明四个service，其中`ServiceA`和`ServiceC`都各自另开一个进程，`ServiceB`和`ServiceD`都在主进程中，AndroidManifest.xml如下：

```

        <service android:name=".ServiceA"
                    android:process="com.hujiang.test.servicea"
                 android:exported="true"/>
        
        <service android:name=".ServiceB"
                 android:exported="false"/>
        
        <service android:name=".ServiceC"
                 android:process="com.hujiang.test.servicec"
            android:exported="true"/>

        <service android:name=".ServiceD"
                 android:exported="false"/>
                 
```

此时在Application中启动四个Service
```

		startService(new Intent(this, ServiceA.class));
        startService(new Intent(this, ServiceB.class));
        startService(new Intent(this, ServiceC.class));
        startService(new Intent(this, ServiceD.class));
        
```
同时各Service打下如下log:

```
 public static final String TAG = ServiceB.class.getSimpleName();
    @Override
    public void onCreate() {
        super.onCreate();
        Log.i(TAG,"onCreate" + "pid：" + android.os.Process.myPid());
    }

    @Override
    public int onStartCommand(Intent intent, int flags, int startId) {
        Log.i(TAG,"onStartCommand" + "pid：" + android.os.Process.myPid());
        return super.onStartCommand(intent, flags, startId);
    }

```

在log中会发现
`onCreate()`方法各执行一遍，这个是正常的，但`onStartCommand()`方法目前执行了三遍，因为共3个进程。

  

## 真相

1. N个进程，N个独立的虚拟机,Application被N次初使化
2. 处理时应该在Application中分进程初始化数据


<!--more-->
## 剑谱


如下解决方案 

     mProcessName = getCurrentProcessName(this);
        Log.i(TAG, "onCreate" + "getProcessName：" + mProcessName);
        Log.i(TAG, "init_all_process");
        if(TextUtils.equals(mProcessName, getPackageName())){
            Log.i(TAG, "init_main_process");
         } else if(TextUtils.equals(getProcessName(this, android.os.Process.myPid()), "com.hujiang.test.servicea")){
            Log.i(TAG, "init_a_process");
         }else if(TextUtils.equals(getProcessName(this, android.os.Process.myPid()), "com.hujiang.test.servicec")){
            Log.i(TAG, "init_c_process");
         }  
         
获取当前进程名称：


		private String getCurrentProcessName(Context context) {
        int pid = android.os.Process.myPid();
        ActivityManager mActivityManager = (ActivityManager) context
                .getSystemService(Context.ACTIVITY_SERVICE);
        for (ActivityManager.RunningAppProcessInfo appProcess : mActivityManager
                .getRunningAppProcesses()) {
            if (appProcess.pid == pid) {
                return appProcess.processName;
            }
        }
        return null;
    }
    


分别在自己的进程中初始化










