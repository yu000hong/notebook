# RecyclerView实现聊天界面功不能滑动到指定位置问题

RecyclerView不同于listView，其方法scrollToPosition无法实现滑动到最底部，查看源码其并没有实现，意外的发现LinearLayoutManager的smoothScrollToPosition方法实现了，于是使用这个方法，但是定位时有滑动动画，而且下拉加载聊天记录时定位到原来的位置依旧有滑动动画效果，RecyclerView的局限性使我似乎已经没有办法了，只好改用listview来实现了。

绝望之中查看LinearLayoutManager的一些set方法，竟意外的发现setReverseLayout方法，反转布局，于是乎使用这一句代码，并把聊天数据集合顺序也反转，即不用倒序，完美的解决了这一问题。

代码如下：

```java
layoutManager = new LinearLayoutManager(context);
layoutManager.setOrientation(LinearLayoutManager.VERTICAL);
layoutManager.setReverseLayout(true);
```

# 链接

[RecyclerView实现聊天界面功不能滑动到指定位置问题](http://www.lai18.com/content/2481253.html)
