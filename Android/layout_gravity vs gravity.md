# layout_gravity vs gravity

### Question

I know we can set the following value to the **`android:gravity`** and **`android:layout_gravity`**.

- center
- center_vertical
- center_horizental, etc

But I am confused regarding both of these.

What is the difference between the usage of `android:gravity` and `android:layout_gravity`?

### Answer

Their names should help you:

- `android:gravity` sets the gravity of the content of the `View` its used on.
- `android:layout_gravity` sets the gravity of the `View` or `Layout` in its parent.

And an example is [here](http://thinkandroid.wordpress.com/2010/01/14/how-to-position-views-properly-in-layouts/).

# 链接
[Gravity and layout_gravity on Android](http://stackoverflow.com/questions/3482742/gravity-and-layout-gravity-on-android?answertab=votes#tab-top)
