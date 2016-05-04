# Add new item count to icon on button - Android

### Question

I'm developer. I need to implement design shown below. I already have functional app but wonder how to even approach this? Particulary, I'm interested in how to show Number of "New" items under tabs. What I KNOW how to do - is create new icons with red dots and just display them when new stuff available.

But I have no idea how to make those round circles float on top of title AND show number inside. Does anybody have suggestion on what too look for? Samples? Directions?

Second question about separating activities. Should I make control to combine buttons like this and just inflate it on activities? Otherwise I may create tabbed activity but I'm not sure if it's possible to style it to make it look like this.

![](http://i.stack.imgur.com/2JxFz.png)

### Answer

Make your badge a TextView, allowing you to set the numeric value to anything you like by calling setText(). Set the background of the TextView as an XML <shape> drawable, with which you can create a solid or gradient circle with a border. An XML drawable will scale to fit the view as it resizes with more or less text.

**res/drawable/badge_circle.xml:**

```xml
<shape xmlns:android="http://schemas.android.com/apk/res/android"
    android:shape="ring"
    android:useLevel="false"
    android:thickness="9dp"
    android:innerRadius="0dp">

    <solid android:color="#F00" />
    
    <stroke
        android:width="1dip"
        android:color="#FFF" />

    <padding
        android:top="2dp"
        android:bottom="2dp"/>

</shape>
```

You'll have to take a look at how the oval/circle scales with large 3-4 digit numbers, though. If this effect is undesirable, try a rounded rectangle approach like below. With small numbers, the rectangle will still look like a circle as the radii converge together.

**res/drawable/badge_circle.xml:**

```xml
<shape xmlns:android="http://schemas.android.com/apk/res/android"
  android:shape="rectangle">
  <corners
    android:radius="10dip"/>
  <solid
    android:color="#F00" />
  <stroke
    android:width="2dip"
    android:color="#FFF" />
  <padding
    android:left="5dip"
    android:right="5dip"
    android:top="5dip"
    android:bottom="5dip" />
</shape>
```

With the scalable background created, you simply add it to the background of a TextView, like so:

```xml
<TextView
  android:layout_width="wrap_content"
  android:layout_height="wrap_content" 
  android:text="10"
  android:textColor="#FFF"
  android:textSize="16sp"
  android:textStyle="bold"
  android:background="@drawable/badge_circle"/>
```

Finally, these TextView badges can be placed in your layout on top of the appropriate buttons/tabs. I would probably do this by grouping each button with its badge in a RelativeLayout container, like so:

```xml
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
  android:layout_width="wrap_content"
  android:layout_height="wrap_content">
  <Button
    android:id="@+id/myButton"
    android:layout_width="65dip"
    android:layout_height="65dip"/>
  <TextView
    android:id="@+id/textOne"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:layout_alignTop="@id/myButton"
    android:layout_alignRight="@id/myButton" 
    android:text="10"
    android:textColor="#FFF"
    android:textSize="16sp"
    android:textStyle="bold"
    android:background="@drawable/badge_circle"/>
</RelativeLayout>
```

Hopefully that's enough information to at least get you pointed in the right direction!

### Android ViewBadger Library

A simple way to "badge" any given Android view at runtime without having to cater for it in layout.

Add .jar file in your libs folder

[Click to download](https://github.com/jgilfelt/android-viewbadger/archive/master.zip) Example

see [Example](https://github.com/jgilfelt/android-viewbadger) on github

Simple example:

```java
View target = findViewById(R.id.target_view);
BadgeView badge = new BadgeView(this, target);
badge.setText("1");
badge.show();
```

# 链接

[Add new item count to icon on button - Android](http://stackoverflow.com/questions/6011786/add-new-item-count-to-icon-on-button-android)
