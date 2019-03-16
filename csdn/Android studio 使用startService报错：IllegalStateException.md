# Android 8.0启动service报错：java.lang.RuntimeException和java.lang.IllegalStateException

## 错误信息：

```
 java.lang.RuntimeException: Unable to start receiver cn.edu.zut.broadservice.MyReceiver: java.lang.IllegalStateException: Not allowed to start service Intent { cmp=cn.edu.zut.broadservice/.MyService }: app is in background uid UidRecord{b67c471 u0a86 RCVR idle change:uncached procs:1 seq(0,0,0)}
        at android.app.ActivityThread.handleReceiver(ActivityThread.java:3388)
        at android.app.ActivityThread.access$1200(ActivityThread.java:199)
        at android.app.ActivityThread$H.handleMessage(ActivityThread.java:1661)
        at android.os.Handler.dispatchMessage(Handler.java:106)
        at android.os.Looper.loop(Looper.java:193)
        at android.app.ActivityThread.main(ActivityThread.java:6669)
        at java.lang.reflect.Method.invoke(Native Method)
        at com.android.internal.os.RuntimeInit$MethodAndArgsCaller.run(RuntimeInit.java:493)
        at com.android.internal.os.ZygoteInit.main(ZygoteInit.java:858)
     Caused by: java.lang.IllegalStateException: Not allowed to start service Intent { cmp=cn.edu.zut.broadservice/.MyService }: app is in background uid UidRecord{b67c471 u0a86 RCVR idle change:uncached procs:1 seq(0,0,0)}
        at android.app.ContextImpl.startServiceCommon(ContextImpl.java:1577)
        at android.app.ContextImpl.startService(ContextImpl.java:1532)
        at android.content.ContextWrapper.startService(ContextWrapper.java:664)
        at android.content.ContextWrapper.startService(ContextWrapper.java:664)
        at cn.edu.zut.broadservice.MyReceiver.onReceive(MyReceiver.java:24)
        at android.app.ActivityThread.handleReceiver(ActivityThread.java:3379)
        at android.app.ActivityThread.access$1200(ActivityThread.java:199) 
        at android.app.ActivityThread$H.handleMessage(ActivityThread.java:1661) 
        at android.os.Handler.dispatchMessage(Handler.java:106) 
        at android.os.Looper.loop(Looper.java:193) 
        at android.app.ActivityThread.main(ActivityThread.java:6669) 
        at java.lang.reflect.Method.invoke(Native Method) 
        at com.android.internal.os.RuntimeInit$MethodAndArgsCaller.run(RuntimeInit.java:493) 
        at com.android.internal.os.ZygoteInit.main(ZygoteInit.java:858) 
```

## 错误原因：

- 如果针对 Android 8.0 的应用尝试在不允许其创建后台服务的情况下使用 `startService()` 函数，则该函数将引发一个 `IllegalStateException`。
- 新的 `Context.startForegroundService()` 函数将启动一个前台服务。现在，即使应用在后台运行，系统也允许其调用 `Context.startForegroundService()`。不过，应用必须在创建服务后的五秒内调用该服务的 `startForeground()` 函数。
- 如需了解详细信息，请参阅[后台执行限制](https://developer.android.com/preview/features/background.html?hl=zh-cn)。

## 解决方案：

在 原来启动service的地方，把startService()；替换为下面的代码。

```Android
Intent intent = new Intent(context, MyService.class);
if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.O) {
    context.startForegroundService(intent);
} else {
   context.startService(intent);
}
```

并且在service的类里面加入（若加入上面代码后正常运行可忽略此内容）

```
@Override
public void onCreate() {
    super.onCreate();
   if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.O) {
       startForeground(1,new Notification()); 
}
```



参考 ：[参考地址](https://stackoverflow.com/questions/46445265/android-8-0-java-lang-illegalstateexception-not-allowed-to-start-service-inten)