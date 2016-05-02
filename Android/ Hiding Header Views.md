#  Hiding Header Views

Android [ListView#addHeaderView and ListView#addFooterView](http://developer.android.com/reference/android/widget/ListView.html#addHeaderView(android.view.View, java.lang.Object, boolean)
methods are strange: you have to add the header and footer Views before you set the ListView‘s adapter so the 
ListView can take the headers and footers into consideration — you get an exception otherwise. Here we add a ProgressBar 
(spinner) as the headerView:

```java
// spinner is a ProgressBar
listView.addHeaderView(spinner);
```

We’d like to be able to show and hide that spinner at will, but removing it outright is dangerous because we’d 
never be able to add it again without destroying the ListView — remember, we can’t addHeaderView after 
we’ve it’s adapter:

```java
listView.removeHeaderView(spinner); //dangerous!
```

So let’s hide it! Turns out that’s hard, too. Just hiding the spinner view itself results in an empty, 
but still visible, header area.

![](http://assets.pivotallabs.com/1029/original/spinner.jpg)

Now try to hide the spinner:

```java
spinner.setVisibility(View.GONE);
```

Result: header area still visible with an ugly space:

![](http://assets.pivotallabs.com/1030/original/space.jpg)

The solution is to put the progress bar in a LinearLayout that wraps it’s content, and hiding the content. 
That way the wrapping LinearLayout will collapse when its content is hidden, resulting in a headerView that 
is technically still present, but 0dip high:

```xml
  <LinearLayout
      xmlns:a="http://schemas.android.com/apk/res/android"
      android:orientation="vertical"
      android:layout_width="fill_parent"
      android:layout_height="wrap_content">
    <!-- simplified -->
      <ProgressBar
        android:id="@+id/spinner"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"/>
  </LinearLayout>
```

Then, set the layout as the header:

```java
spinnerLayout = getLayoutInflater().inflate(R.layout.header_view_spinner, null);
listView.addHeaderView(spinnerLayout);
```

And when we need to hide it, hide the layout’s content, not the layout:

```java
spinnerLayout.findViewById(R.id.spinner).setVisibility(View.GONE);
```

Now the header disappears from view. No more ugly space at the top!

# 链接

[Hiding Header Views](https://blog.pivotal.io/pivotal-labs/labs/android-tidbits-6-22-2011-hiding-header-views)
