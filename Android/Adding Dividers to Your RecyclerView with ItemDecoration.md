# Adding Dividers to Your RecyclerView with ItemDecoration

At Google I/O 2014, Google unveiled `RecyclerView`, a successor to `ListView`. RecyclerView sought to strip away much of 
the cruft that slowed down ListView, and in doing so, shipped without some of the conveniences we’d gotten used to. 
We’ve discussed how to responsibly add some of that functionality back in [previous blog posts](https://www.bignerdranch.com/blog/?q=recyclerview),
and today we’ll continue with a look at list dividers.

One common task that isn’t as convenient in RecyclerView is the addition of dividers or offsets between list items, 
but you can fortunately create your own dividers with `ItemDecoration`.

## ItemDecoration

RecyclerView.ItemDecoration is a tool used to decorate the children of a RecyclerView. RecyclerView is a ViewGroup, 
with each of its children representing an item in a list. With ItemDecoration you can easily modify the appearance of 
these child views.

For example, let’s add dividers to a RecyclerView using a `LinearLayoutManager`, oriented vertically. In order to do so, 
we’ll subclass ItemDecoration, giving our new class a constructor that takes in a divider to be drawn between list items.

```java
public class DividerItemDecoration extends RecyclerView.ItemDecoration {

    private Drawable mDivider;

    public DividerItemDecoration(Drawable divider) {
        mDivider = divider;
    }
...
```

ItemDecoration features three methods for us to override, but we’ll only need two: `getItemOffsets` and `onDraw`.

First, getItemOffsets. We need to provide offsets between list items so that we’re not drawing dividers on top of our 
child views. getItemOffsets is called for each child of your RecyclerView.

```java
@Override
public void getItemOffsets(Rect outRect, View view, RecyclerView parent, RecyclerView.State state) {
    super.getItemOffsets(outRect, view, parent, state);

    if (parent.getChildAdapterPosition(view) == 0) {
        return;
    }

    outRect.top = mDivider.getIntrinsicHeight();
}
```

> Not completed, please read original article

# 链接

[Adding Dividers to Your RecyclerView with ItemDecoration](https://www.bignerdranch.com/blog/a-view-divided-adding-dividers-to-your-recyclerview-with-itemdecoration/)


