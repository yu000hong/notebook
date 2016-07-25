# 6 ways to make your lists scroll faster than the wind

There are good times and there are bad times. Making an app is always fun. What's not fun, is getting a brand new MotoX and watching your app run slower on it that the sad old samsung testing phone in office. It literally had me in tears the first time I saw it. And even though it was late in the evening, I decided to find out what I can do to fix it.

Here's a quick list of what I have found:

1. Reduce the number of conditions used in the getView of your adapter.

2. Check and reduce the number of garbage collection warnings that you get in the logs

3. If you're loading images while scrolling, get rid of them

4. Set scrollingCache and animateCache to false (more on this later)

5. Simplify the hierarchy of the list view row layout

6. Use the view holder pattern

If you have already done all of these, you can probably stop reading now. However, if you haven't, read on. (A bit of caution here. What we are suggesting here is not a single fix. Sometimes, one of these will help, sometimes you would need to try out a lot of things. It really depends on your code. The aim here to collect a set of rules which helps things scroll faster.)

### 1. Reduce the number of conditions used in the getView of your adapter.

Your getView adapter needs to be clean. This class gets called every time a row shows up on your screen. So, every extra instruction set that you define here, gets called hundreds of times when the app runs. Hence it makes sense to reduce the number of things that you do here.

Now the problem is that if you can't do it here, where do you do it. The answer is simple. If you are using a serializable class to get your values, then you move each condition to the main class which is setting the values in the serializable class. This way, the conditions are checked only once. Here's a simple example (drop in a comment if you need more info).

Here is the old sulky list view. (There were more complicated conditions, but we removed them to make it easier to understand)

```java
@Override
public View getView(int position, View convertView, ViewGroup paramViewGroup) {
    Object current_event = mObjects.get(position);
    ViewHolder holder = null;
    if (convertView == null) {
        holder = new ViewHolder();
        convertView = inflater.inflate(R.layout.row_event, null);
        holder.ThreeDimension = (ImageView) convertView.findViewById(R.id.ThreeDim);
        holder.EventPoster = (ImageView) convertView.findViewById(R.id.EventPoster);
        convertView.setTag(holder);
    } else {
            holder = (ViewHolder) convertView.getTag();
    }

    // If you have conditions like below then you may face problem
    if (doesSomeComplexChecking()) {
            holder.ThreeDimention.setVisibility(View.VISIBLE);
    } else {
            holder.ThreeDimention.setVisibility(View.GONE); 
    }

    // This method is setting layout parameters each time when getView is getting called which is wrong.
    RelativeLayout.LayoutParams imageParams = new RelativeLayout.LayoutParams(measuredwidth, rowHeight);
    holder.EventPoster.setLayoutParams(imageParams);

    return convertView;
}
```

This is the new and updated getView

```java
@Override
public View getView(int position, View convertView, ViewGroup paramViewGroup) {
    Object current_event = mObjects.get(position);
    ViewHolder holder = null;

    if (convertView == null) {
        holder = new ViewHolder();
        convertView = inflater.inflate(R.layout.row_event, null);
        holder.ThreeDimension = (ImageView) convertView.findViewById(R.id.ThreeDim);
        holder.EventPoster = (ImageView) convertView.findViewById(R.id.EventPoster);
        // This will now execute only for the first time of each row
        RelativeLayout.LayoutParams imageParams = new RelativeLayout.LayoutParams(measuredwidth, rowHeight);
        holder.EventPoster.setLayoutParams(imageParams);
        convertView.setTag(holder);
    } else {
        holder = (ViewHolder) convertView.getTag();
    }

    // This way you can simply avoid conditions
    holder.ThreeDimension.setVisibility(object.getVisibility());

    return convertView;
}
```

And BAM you're done.

### 2. Check and reduce the number of garbage collection warnings that you get in the logs

GC occurs frequently when you're creating lots of objects and then destroying them. So the general advice here would be to not create a lot of objects in getView(), and an even better advice - to not create objects at all except the view holder. If you are seeing "GC has freed some memory" message in your Log VERY frequently then you are doing something grievously wrong. You will have take a look at one of the following

- Hierarchy of list row layout
- Conditions in your getView adapter
- List view properties in layout

### 3. If you're loading images while scrolling, get rid of them

When a user fires a fling MotionEvent , application is downloading images, it needs to stop, concentrate of scrolling, and then get back to downloading images again as soon as motion stops. The effects of doing this is almost magical.

Also, Google gave out code to show how it's done. You can find it here - [https://code.google.com/p/iosched/](https://code.google.com/p/iosched/) (I would really love it if google could post stuff on github and not Google Code)

For those of you who hate opening links and downloading codes, here's a quick snippet of how do you add a scroll listener to stop and start the queue.

```java
listView.setOnScrollListener(new OnScrollListener() {
    @Override
    public void onScrollStateChanged(AbsListView listView, int scrollState) {
        // Pause disk cache access to ensure smoother scrolling
        if (scrollState == AbsListView.OnScrollListener.SCROLL_STATE_FLING) {
            imageLoader.stopProcessingQueue();
        } else {
            imageLoader.startProcessingQueue();
        }
    }

    @Override
    public void onScroll(AbsListView view, int firstVisibleItem, int visibleItemCount, int totalItemCount) {
            // TODO Auto-generated method stub
    }
});
```

### 4. Set scrollingCache and animateCache to false

For the uninitiated, these are properies of the list view. Here are some more details from Numan Salati on StackOverflow (http://stackoverflow.com/questions/15570041/scrollingcache)

> Scrolling cache is basically a drawing cache.

> In android, you can ask a View to store its drawing in a cache called drawing cache (basically a bitmap). By default, a drawing cache is disabled because it takes up memory but you can ask the View to explicitly to create one either via setDrawingCacheEnabled or through hardware layers (setLayerType).

> So why is it useful? Because using a drawing cache make your animation smooth compared to redrawing the view at every frame.

> This type of animation can also be hardware accelerated because the rendering system can take this bitmap and upload it to the GPU as a texture (if using hardware layers) and do fast matrix manipulations on it (like change alpha, translate, rotation). Compare that to doing animation were you are redrawing (onDraw gets called) on every frame.

> In the case of a listview, when you scroll by flinging, you are in essence animating the views of your list (either moving them up or down). The listview uses the drawing cache of its visible children (and some potentially visible children near the edges) to animate them very quickly.

> Is there a disadvantage to using drawing cache? Yes it consumes memory which is why by default it is turned off for in a View. In the case of ListView, the cache automatically created for you as soon as you touch the ListView and move a little (to differentiate a tap from scroll). In other words, as soon as ListView thinks you are about to scroll/fling it will create a scroll cache for you to animate the scroll/fling motion.

Similary, AnimationCache can be evil sometimes (by evil I mean it increases the frequency of GC)

> Defines whether layout animations should create a drawing cache for their children. Enabling the animation cache consumes more memory and requires a longer initialization but provides better performance. The animation cache is enabled by default.

[more details](http://developer.android.com/reference/android/view/ViewGroup.html#attr_android:animationCache)

Here's what we did to our list view. First, here's the original list view.

```xml
<ListView
    android:id="@android:id/list"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:divider="@color/list_background_color"
    android:dividerHeight="0dp"
    android:listSelector="#00000000"
    android:smoothScrollbar="true" /> 
```

And here's the one after the change

```xml
<ListView
    android:id="@android:id/list"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:divider="@color/list_background_color"
    android:dividerHeight="0dp"
    android:listSelector="#00000000"
    android:smoothScrollbar="true"
    android:scrollingCache="false"
    android:animationCache="false" />
```

### 5. Simplify the hierarchy of the list view row layout

You NEED to keep the hierarchy of the list view row layout as small as possible. The hierarchy or the depth is directly related to the Measuring and Drawing time of the layout (which you can find in the Hierarchy Viewer tool). We'll probably cover tricks to do these in a later post.

### 6. Use the view holder pattern

This is definitely a beginner's mistake. The first time you do this right, your app will automatically end up in FastScroll Land. The reason this happens is because by doing this when the list scrolls, the layout is inflated only the first time. You can find more about this [here](http://www.vogella.com/tutorials/AndroidListView/article.html#adapterperformance_hoder) and [here](http://www.javacodegeeks.com/2013/09/android-viewholder-pattern-example.html)

Are there any other tricks that I have missed out?

# LINK

[6 ways to make your lists scroll faster than the wind](https://leftshift.io/6-ways-to-make-your-lists-scroll-faster-than-the-wind)

