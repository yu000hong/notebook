# How to get width&height of a View

As the idea of the Android evolved, Android has received wide attention and deployed on a very wide range of devices. 
Android UI had to move and make developers' life easier: AbsoluteLayout got deprecated. It is very logical because 
your app will be installed on very small devices and very large devices and all the devices in between.

Now, it is all `WRAP_CONTENT`, `FILL_PARENT`/`MATCH_PARENT`. Yet, a developer sometimes needs to know the dimensions
of his view to do some extra tweaks to perfect his ui.

So, what is the best way to do so?

Well, there are several ways of getting the dimensions of a view. Most of them boil down to waiting the layout of the 
view hierarchy. In case you still have not gotten into the problem of `getWidth()` and `getHeight()` returning 0, 
well **that is very normal in Android as the width and height of a view are zero until they are visible, measured, 
and part of the layout.**

In the following I will assume that your View is called view.

### Using the famous `OnGlobalLayoutListener`

This is one of the most used mechanisms to get the view dimensions. You attach a [Global Layout Listener](http://developer.android.com/reference/android/view/ViewTreeObserver.OnGlobalLayoutListener.html)
to the view hierarchy. It helps you actually get the width of all the views in your view heirarchy: 

```java
view.getViewTreeObserver().addOnGlobalLayoutListener(new OnGlobalLayoutListener() {
  @SuppressLint("NewApi")
  @SuppressWarnings("deprecation")
  @Override
  public void onGlobalLayout() {
    //now we can retrieve the width and height
    int width = view.getWidth();
    int height = view.getHeight();
    //...
    //do whatever you want with them
    //...
    //this is an important step not to keep receiving callbacks:
    //we should remove this listener
    //I use the function to remove it based on the api level!
    if(android.os.Build.VERSION.SDK_INT >= android.os.Build.VERSION_CODES.JELLY_BEAN)
      view.getViewTreeObserver().removeOnGlobalLayoutListener(this);
    else
      view.getViewTreeObserver().removeGlobalOnLayoutListener(this);
  }
});
```
 
### By extending a View
 
Personally, I use this approach when the whole Activity or Layout is really dependant on one view only, 
most probably, a GridView or a ListView. I have already presented this solution on one SO question: 
[How to manage GridView](http://stackoverflow.com/questions/13681024/how-to-manage-gridview/13835812#13835812).
The whole idea is to instantiate your view in your code instead of inserting it in the layout xml, and by doing this, 
you are able to override the `onLayout` function.
 
```java
mGrid = new GridView(this) {
    @Override
    protected void onLayout(boolean changed, int l, int t, int r, int b) {
      super.onLayout(changed, l, t, r, b);
      //here you have the size of the view and you can do stuff
    }
};
```

### By forcing a measurement of the View

This way is probably rarely used but I personally like it. I am not 100% sure but this probably is the most 
**lightweight** of them all, although it might not be as precise. If you force the view to measure itself, 
you can get the measured dimensions. I use this when you need to move some views (using their margins) based 
on other views when you can not simply use some of the layout properties because they have different parents 
or something. Here is the way to do it:

```java
view.measure(MeasureSpec.UNSPECIFIED, MeasureSpec.UNSPECIFIED);
int widht = view.getMeasuredWidth();
int height = view.getMeasuredHeight();
```

# 链接

[# How to get width&height of a View](http://www.sherif.mobi/2013/01/how-to-get-widthheight-of-view.html)
