# Using the RecyclerView

### Overview

The `RecyclerView` is a new ViewGroup that is prepared to render any adapter-based view in a similar way. 
It is supposed to be the successor of `ListView` and `GridView`, and it can be found in the latest support-v7 version. 
One of the reasons is that **RecyclerView has a more extensible framework, especially since it provides the ability to 
implement both horizontal and vertical layouts**. Use the RecyclerView widget when you have data collections whose 
elements change at runtime based on user action or network events.

If you want to use a RecyclerView, you will need to work with the following:

- **`RecyclerView.Adapter`** - To handle the data collection and bind it to the view
- **`LayoutManager`** - Helps in positioning the items
- **`ItemAnimator`** - Helps with animating the items for common operations such as Addition or Removal of item

![](https://developer.android.com/training/material/images/RecyclerView.png)

Furthermore, it provides animation support for ListView items whenever they are added or removed, which had been 
extremely difficult to do in the current implementation. RecyclerView also begins to enforce the `ViewHolder pattern` too, 
which was already a recommended practice but now deeply integrated with this new framework.

For more details, see [this detailed overview](http://www.grokkingandroid.com/first-glance-androids-recyclerview/).

### Compared to ListView

RecyclerView differs from its predecessor ListView primarily because of the following features:

- **`Required ViewHolder in Adapters`** - ListView adapters do not require the use of the ViewHolder pattern to improve 
performance. In contrast, implementing an adapter for RecyclerView requires the use of the ViewHolder pattern.

- **`Customizable Item Layouts`** - ListView can only layout items in a vertical linear arrangement and this cannot be 
customized. In contrast, the RecyclerView has a RecyclerView.LayoutManager that allows any item layouts including 
horizontal lists or staggered grids.

- **`Easy Item Animations`** - ListView contains no special provisions through which one can animate the addition or 
deletion of items. In contrast, the RecyclerView has the RecyclerView.ItemAnimator class for handling item animations.

- **`Manual Data Source`** - ListView had adapters for different sources such as ArrayAdapter and CursorAdapter for 
arrays and database results respectively. In contrast, the RecyclerView.Adapter requires a custom implementation to 
supply the data to the adapter.

- **`Manual Item Decoration`** - ListView has the `android:divider` property for easy dividers between items in the list. 
In contrast, RecyclerView requires the use of a RecyclerView.ItemDecoration object to setup much more manual divider 
decorations.

- **`Manual Click Detection`** - ListView has a AdapterView.OnItemClickListener interface for binding to the click 
events for individual items in the list. In contrast, RecyclerView only has support for RecyclerView.OnItemTouchListener 
which manages individual touch events but has no built-in click handling.

### Components of a RecyclerView

#### LayoutManagers

A RecyclerView needs to have a layout manager and an adapter to be instantiated. A layout manager positions item views 
inside a RecyclerView and determines when to reuse item views that are no longer visible to the user.

RecyclerView provides these built-in layout managers:

- **`LinearLayoutManager`** shows items in a vertical or horizontal scrolling list.

- **`GridLayoutManager`** shows items in a grid.

- **`StaggeredGridLayoutManager`** shows items in a staggered grid.

To create a custom layout manager, extend the [`RecyclerView.LayoutManager`](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.LayoutManager.html) class.

#### RecyclerView.Adapter

RecyclerView includes a new kind of adapter. It’s a similar approach to the ones you already used, but with some 
peculiarities, such as a required ViewHolder. You will have to override two main methods: 

- one to inflate the view and its view holder, 

- and another one to bind data to the view. 

The good thing about this is that first method is called only when we really need to create a new view. 
No need to check if it’s being recycled.

#### ItemAnimator

RecyclerView.ItemAnimator will animate ViewGroup modifications such as add/delete/select that are notified to adapter. 
DefaultItemAnimator can be used for basic default animations and works quite well. See the 
[section](https://guides.codepath.com/android/Using-the-RecyclerView#animators) of this guide for more information.

### Using the RecyclerView

Using a RecyclerView has the following key steps:

- Add RecyclerView support library to the gradle build file
- Define a model class to use as the data source
- Add a RecyclerView to your activity to display the items
- Create a custom row layout XML file to visualize the item
- Create a RecyclerView.Adapter and ViewHolder to render the item
- Bind the adapter to the data source to populate the RecyclerView

The steps are explained in more detail below.

#### Installation

Make sure the recyclerview support library is listed as a dependency in your `app/build.gradle`:

```
dependencies {
    ...
    compile 'com.android.support:recyclerview-v7:23.2.1'
}
```

Click on "Sync Project with Gradle files" to let your IDE download the appropriate resources.

#### Defining a Model

Every RecyclerView is backed by a source for data. In this case, we will define a Contact class which represents the data model being displayed by the RecyclerView:

```java
public class Contact {
    private String mName;
    private boolean mOnline;

    public Contact(String name, boolean online) {
        mName = name;
        mOnline = online;
    }

    public String getName() {
        return mName;
    }

    public boolean isOnline() {
        return mOnline;
    }

    private static int lastContactId = 0;

    public static ArrayList<Contact> createContactsList(int numContacts) {
        ArrayList<Contact> contacts = new ArrayList<Contact>();

        for (int i = 1; i <= numContacts; i++) {
            contacts.add(new Contact("Person " + ++lastContactId, i <= numContacts / 2));
        }

        return contacts;
    }
}
```

#### Create the RecyclerView within Layout

Inside the desired activity layout XML file in `res/layout/activity_users.xml`, let's add the RecyclerView from the support library:

```xml
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent" >
    
    <android.support.v7.widget.RecyclerView
      android:id="@+id/rvContacts"
      android:layout_width="match_parent"
      android:layout_height="match_parent" />

</RelativeLayout>
```

In the layout, preview we can see the RecyclerView within the activity:

![](https://i.imgur.com/Qf5fQ8X.png)

Now the RecyclerView is embedded within our activity layout file. Next, we can define the layout for each item within our list.

#### Creating the Custom Row Layout

Before we create the adapter, let's define the XML layout file that will be used for each row within the list. This item layout for now should contain a horizontal linear layout with a textview for the name and a button to message the person:

![](https://i.imgur.com/wPRTc76.png) ![](https://i.imgur.com/fu3FzsV.png)

This layout file can be created in `res/layout/item_contact.xml` and will be rendered for each item row. Note that you should be using wrap_content for the layout_height because RecyclerView versions prior to 23.2.1 previously ignored layout parameters. See [this link](http://android-developers.blogspot.com/2016/02/android-support-library-232.html) for more context.

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
        xmlns:android="http://schemas.android.com/apk/res/android"
        android:orientation="horizontal"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:paddingTop="10dp"
        android:paddingBottom="10dp">

    <TextView
        android:id="@+id/contact_name"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:layout_weight="1"
        />

    <Button
        android:id="@+id/message_button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:paddingLeft="16dp"
        android:paddingRight="16dp"
        android:textSize="10sp"
        />
</LinearLayout>
```

With the custom item layout complete, let's create the adapter to populate the data into the recycler view.
