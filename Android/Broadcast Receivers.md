# Broadcast Receivers

> **`Broadcast Receivers`** simply respond to broadcast messages from other applications or from the system itself. 
> These messages are sometime called events or intents. For example, applications can also initiate broadcasts to 
> let other applications know that some data has been downloaded to the device and is available for them to use, 
> so this is broadcast receiver who will intercept this communication and will initiate appropriate action.

There are following two important steps to make `BroadcastReceiver` works for the system broadcasted intents 

- Creating the Broadcast Receiver.

- Registering Broadcast Receiver

There is one additional steps in case you are going to implement your custom intents then you will have to create and 
broadcast those intents.

### Creating the Broadcast Receiver

A broadcast receiver is implemented as a subclass of `BroadcastReceiver` class and overriding the `onReceive()` method 
where each message is received as a Intent object parameter.

```java
public class MyReceiver extends BroadcastReceiver {
   @Override
   public void onReceive(Context context, Intent intent) {
      Toast.makeText(context, "Intent Detected.", Toast.LENGTH_LONG).show();
   }
}
```

### Registering Broadcast Receiver

An application listens for specific broadcast intents by registering a broadcast receiver in `AndroidManifest.xml` file. 
Consider we are going to register MyReceiver for system generated event `ACTION_BOOT_COMPLETED` which is fired 
by the system once the Android system has completed the boot process.

![](http://www.tutorialspoint.com/android/images/broadcast.jpg)

```xml
<application
   android:icon="@drawable/ic_launcher"
   android:label="@string/app_name"
   android:theme="@style/AppTheme" >
   <receiver android:name="MyReceiver">
   
      <intent-filter>
         <action android:name="android.intent.action.BOOT_COMPLETED">
         </action>
      </intent-filter>
   
   </receiver>
</application>
```

Now whenever your Android device gets booted, it will be intercepted by BroadcastReceiver MyReceiver and implemented 
logic inside onReceive() will be executed.

There are several system generated events defined as final static fields in the `Intent` class. The following table lists 
a few important system events.

|Event Constant|Description|
|--------------|-----------|
|android.intent.action.BATTERY_CHANGED|Sticky broadcast containing the charging state, level, and other information about the battery.|
|android.intent.action.BATTERY_LOW|Indicates low battery condition on the device.|
|android.intent.action.BATTERY_OKAY|Indicates the battery is now okay after being low.|
|android.intent.action.BOOT_COMPLETED|This is broadcast once, after the system has finished booting.|
|android.intent.action.BUG_REPORT|Show activity for reporting a bug.|
|android.intent.action.CALL|Perform a call to someone specified by the data.|
|android.intent.action.CALL_BUTTON|The user pressed the "call" button to go to the dialer or other appropriate UI for placing a call.|
|android.intent.action.DATE_CHANGED|The date has changed.|
|android.intent.action.REBOOT|Have the device reboot.|

### Broadcasting Custom Intents

If you want your application itself should generate and send custom intents then you will have to create and send 
those intents by using the `sendBroadcast()` method inside your activity class. If you use the `sendStickyBroadcast(Intent)` 
method, the Intent is sticky, meaning the Intent you are sending stays around after the broadcast is complete.

```java
public void broadcastIntent(View view){
   Intent intent = new Intent();
   intent.setAction("com.tutorialspoint.CUSTOM_INTENT");
   sendBroadcast(intent);
}
```

This intent com.tutorialspoint.CUSTOM_INTENT can also be registered in similar way as we have regsitered system 
generated intent.

```xml
<application
   android:icon="@drawable/ic_launcher"
   android:label="@string/app_name"
   android:theme="@style/AppTheme" >
   <receiver android:name="MyReceiver">
   
      <intent-filter>
         <action android:name="com.tutorialspoint.CUSTOM_INTENT">
         </action>
      </intent-filter>
   
   </receiver>
</application>
```

### Example

This example will explain you how to create BroadcastReceiver to intercept custom intent. Once you are familiar with 
custom intent, then you can program your application to intercept system generated intents. So let's follow the 
following steps to modify the Android application we created in Hello World Example chapter.

|Step|Description|
|----|-----------|
|1|You will use Android studio to create an Android application and name it as My Application under a package com.example.My Application as explained in the Hello World Example chapter.|
|2|Modify main activity file MainActivity.java to add broadcastIntent() method.|
|3|Create a new java file called MyReceiver.java under the package com.example.My Application to define a BroadcastReceiver.|
|4|An application can handle one or more custom and system intents without any restrictions. Every indent you want to intercept must be registered in your AndroidManifest.xml file using <receiver.../> tag|
|5|Modify the default content of res/layout/activity_main.xml file to include a button to broadcast intent.|
|6|No need to modify the string file, Android studio take care of string.xml file.|
|7|Run the application to launch Android emulator and verify the result of the changes done in the application.|

Following is the content of the modified main activity file src/com.example.My Application/MainActivity.java. 
This file can include each of the fundamental life cycle methods. We have added broadcastIntent() method to broadcast a 
custom intent.

```java
package com.example.My Application;

import android.os.Bundle;
import android.app.Activity;
import android.view.Menu;
import android.content.Intent;
import android.view.View;

public class MainActivity extends Activity {

   @Override
   public void onCreate(Bundle savedInstanceState) {
      super.onCreate(savedInstanceState);
      setContentView(R.layout.activity_main);
   }
   
   @Override
   public boolean onCreateOptionsMenu(Menu menu) {
      getMenuInflater().inflate(R.menu.activity_main, menu);
      return true;
   }
   
   // broadcast a custom intent. 
   public void broadcastIntent(View view){
      Intent intent = new Intent();
      intent.setAction("com.tutorialspoint.CUSTOM_INTENT");
      sendBroadcast(intent);
   }
}
```

Following is the content of src/com.example.MyApplication/MyReceiver.java:

```java
package com.example.My Application;

import android.content.BroadcastReceiver;
import android.content.Context;
import android.content.Intent;
import android.widget.Toast;

public class MyReceiver extends BroadcastReceiver {
   @Override
   public void onReceive(Context context, Intent intent) {
      Toast.makeText(context, "Intent Detected.", Toast.LENGTH_LONG).show();
   }
}
```

Following will the modified content of AndroidManifest.xml file. Here we have added <service.../> tag to include our service:

```xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
   package="com.example.My Application"
   android:versionCode="1"
   android:versionName="1.0" >
   
   <uses-sdk
      android:minSdkVersion="8"
      android:targetSdkVersion="22" />
   
   <application
       android:icon="@drawable/ic_launcher"
       android:label="@string/app_name"
       android:theme="@style/AppTheme" >
       
       <activity
          android:name=".MainActivity"
          android:label="@string/title_activity_main" >
       
          <intent-filter>
             <action android:name="android.intent.action.MAIN" />
             <category android:name="android.intent.category.LAUNCHER"/>
          </intent-filter>
          
       </activity>
       
       <receiver android:name="MyReceiver">
       
       <intent-filter>
          <action android:name="com.tutorialspoint.CUSTOM_INTENT">
          </action>
       </intent-filter>
       
       </receiver>
       
   </application>
</manifest>
```

Following will be the content of res/layout/activity_main.xml file to include a button to broadcast our custom intent

```xml
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
   xmlns:tools="http://schemas.android.com/tools" android:layout_width="match_parent"
   android:layout_height="match_parent" android:paddingLeft="@dimen/activity_horizontal_margin"
   android:paddingRight="@dimen/activity_horizontal_margin"
   android:paddingTop="@dimen/activity_vertical_margin"
   android:paddingBottom="@dimen/activity_vertical_margin" tools:context=".MainActivity">
   
   <TextView
      android:id="@+id/textView1"
      android:layout_width="wrap_content"
      android:layout_height="wrap_content"
      android:text="Example of Broadcast"
      android:layout_alignParentTop="true"
      android:layout_centerHorizontal="true"
      android:textSize="30dp" />
      
   <TextView
      android:id="@+id/textView2"
      android:layout_width="wrap_content"
      android:layout_height="wrap_content"
      android:text="Tutorials point "
      android:textColor="#ff87ff09"
      android:textSize="30dp"
      android:layout_above="@+id/imageButton"
      android:layout_centerHorizontal="true"
      android:layout_marginBottom="40dp" />
      
   <ImageButton
      android:layout_width="wrap_content"
      android:layout_height="wrap_content"
      android:id="@+id/imageButton"
      android:src="@drawable/abc"
      android:layout_centerVertical="true"
      android:layout_centerHorizontal="true" />
      
   <Button
      android:layout_width="wrap_content"
      android:layout_height="wrap_content"
      android:id="@+id/button2"
      android:text="Broadcast Intent"
      android:onClick="broadcastIntent"
      android:layout_below="@+id/imageButton"
      android:layout_centerHorizontal="true" />

</RelativeLayout>
```

Following will be the content of res/values/strings.xml to define two new constants

```xml
<resources>    
    <string name="menu_settings">Settings</string>
    <string name="title_activity_main">My Application</string>
</resources>
```

Let's try to run our modified Hello World! application we just modified. I assume you had created your AVD 
while doing environment set-up. To run the app from Android studio, open one of your project's activity files 
and click Run Eclipse Run Icon icon from the tool bar. Android Studio installs the app on your AVD and starts it 
and if everything is fine with your set-up and application, it will display following Emulator window

![](http://www.tutorialspoint.com/android/images/broadcast1.jpg)

Now to broadcast our custom intent, let's click on Broadcast Intent button, this will broadcast our custom 
intent "com.tutorialspoint.CUSTOM_INTENT" which will be intercepted by our registered BroadcastReceiver i.e. MyReceiver 
and as per our implemented logic a toast will appear on the bottom of the the simulator as follows

![](http://www.tutorialspoint.com/android/images/broadcast2.jpg)

You can try implementing other BroadcastReceiver to intercept system generated intents like system boot up, date changed, low battery etc.


