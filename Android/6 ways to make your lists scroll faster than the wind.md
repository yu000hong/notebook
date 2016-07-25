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

