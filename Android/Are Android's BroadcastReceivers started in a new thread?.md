# Are Android's BroadcastReceivers started in a new thread?

### Question

If I have an inner class that extends `BroadcastReceiver` within my Service class, should I care about synchronization, 
when the BroadcastReceiver class reads/writes to objects from the Service class? Or to put it in another way: 
Are BroadacstReceiver's `onReceive()` Methods started in an extra thread?

### Answer

> Are Android's BroadcastReceivers started in a new thread?

Usually, it all depends how you register it.

If you register your BroadcastReceiver using:

```java
registerReceiver(BroadcastReceiver receiver, IntentFilter filter)
```

It will run in the main activity thread(aka `UI thread`).

If you register your BroadcastReceiver using a valid `Handler` running on a different thread:

```java
registerReceiver (BroadcastReceiver receiver, 
          IntentFilter filter, 
          String broadcastPermission, 
          Handler scheduler)
```

It will run in the context of your `Handler`

For example:

```java
HandlerThread handlerThread = new HandlerThread("ht");
handlerThread.start();
Looper looper = handlerThread.getLooper();
Handler handler = new Handler(looper);
context.registerReceiver(receiver, filter, null, handler); // Will not run on main thread
```

Details [here](http://developer.android.com/reference/android/content/Context.html#registerReceiver%28android.content.BroadcastReceiver,%20android.content.IntentFilter%29) & 
[here](http://developer.android.com/reference/android/content/Context.html#registerReceiver%28android.content.BroadcastReceiver,%20android.content.IntentFilter%29).
