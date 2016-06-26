# 关于intent之android.intent.action.USER_PRESENT的接收与使用

在做解锁监听程序时，一开始采用监听屏幕`SCREEN_ON`和`SCREEN_OFF`这两个action。
但奇怪的是，这两个action只能通过代码动态的形式注册，才能被监听到，使用AndroidManifest.xml完全监听不到。
百度后发现这是PowerManager那边在发这个广播的时候做了限制，限制只能有register到代码中的receiver才能接收。

后来就找各种能静态注册AndroidManifest.xml同时能反映用户解锁行为的广播，于是找到`android.intent.action.USER_PRESENT`。
每个用户隔一段时间重新开始使用手机时，首先按电源键点亮屏幕，紧接着解锁。`android.intent.action.USER_PRESENT`就是解锁时发出的intent。
于是，监听`android.intent.action.USER_PRESENT`就能识别用户进入home界面，进而启动想启动的相关服务，包括弹出对话框welcome用户、后台启动程序升级服务等等。

AndroidManifest.xml文件中注册代码：
```xml
<receiver android:name=".ActionReceiver">
   <intent-filter android:priority="90000">
   <action android:name="android.intent.action.USER_PRESENT" />
   </intent-filter>
</receiver>
```

这个intent的说明文档是:
> public static final String ACTION_USER_PRESENT

> Since: API Level 3

> Broadcast Action: Sent when the user is present after device wakes up (e.g when the keyguard is gone).

> This is a protected intent that can only be sent by the system.

> Constant Value: "android.intent.action.USER_PRESENT"

翻译过来就是:

`注意这个action只能有系统发出，是在用户唤醒机器的时候才会发出这种action。`

另外, 真的屏幕解锁的时候触发这个广播，而我们接收这个广播会受好多条件的制约。比如有些第三方桌面不会真正的锁屏，360，LBE等安全软件对广播的拦截等等。

# 链接

[http://blog.csdn.net/heikefangxian23/article/details/46888645](关于intent之android.intent.action.USER_PRESENT的接收与使用)
