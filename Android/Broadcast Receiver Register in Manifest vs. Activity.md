# Broadcast Receiver Register in Manifest vs. Activity

### Question

I need some help to understand when I can expect my broadcast receiver will work when just registered in the manifest 
versus having to be registered from a running activity or service. 
So for example if I register a stand alone receiver with the following intent filter it works without having a 
service/activity reference to it:

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.blk_burn.standalonereceiver"
    android:versionCode="1"
    android:versionName="1.0" >

    <uses-sdk android:minSdkVersion="10" />
    <uses-permission android:name="android.permission.WAKE_LOCK"/>

    <application
        android:icon="@drawable/ic_launcher"
        android:label="@string/app_name" >

        <receiver android:name="TestReceiver">
            <intent-filter>
                <action android:name="android.media.AUDIO_BECOMING_NOISY"/>
            </intent-filter>
        </receiver>

    </application>

</manifest>
```

However if I replace android.media.AUDIO_BECOMING_NOISY with android.intent.action.HEADSET_PLUG the receiver 
is not triggered (Android Documentation) From what I found on this site you have to register this receiver 
from an activity or service that is already running for it to work (Post).

Can anyone tell me why this does not work when just adjusting your intent filter in the manifest and why you need 
to have a service running in the background that references/registers the receiver?

Is there a work around so that I can just register my receiver in my app's manifest using an intent filter with 
android.intent.action.HEADSET_PLUG?

How can do I identify which Broadcast actions from the android documentation need to have a service or 
activity register them versus just having the right filter in the manifest?

### Answer

If you receiver is registered in the manifest and your app is not running, a new process will be created to 
handle the broadcast. If you register it in code, it's tied to the life of the activity/service you registered it in. 
For some broadcasts, it doesn't really make sense to create a new app process if it doesn't exist, or there are some 
security, performance, etc. implications, and thus you can only register the receiver in code.

As for the HEADSET_PLUG broadcast, it seems the idea is that your already running app can get this to do app-specific 
adjustments to UI, volume, etc. If your app is not running, you shouldn't really care about the headphones being unplugged.

AFAIK, there is no one place this info is summarized for all broadcasts, but each Intent should have a comment in the 
JavaDoc about how to register and use it, but apparently it's lacking at places. You should be able to compile a list 
if you grep the Android source tree for [`Intent.FLAG_RECEIVER_REGISTERED_ONLY`](http://developer.android.com/reference/android/content/Intent.html#FLAG_RECEIVER_REGISTERED_ONLY).

