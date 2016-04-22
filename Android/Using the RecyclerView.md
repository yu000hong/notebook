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

#### Creating the RecyclerView.Adapter

Here we need to create the adapter which will actually populate the data into the RecyclerView. The adapter's role is to **convert an object at a position into a list row item** to be inserted.

However with a RecyclerView the adapter requires the existence of a "ViewHolder" object which describes and provides access to all the views within each item row. We can create the basic empty adapter and holder together in `ContactsAdapter.java` as follows:

```java
// Create the basic adapter extending from RecyclerView.Adapter
// Note that we specify the custom ViewHolder which gives us access to our views
public class ContactsAdapter extends 
    RecyclerView.Adapter<ContactsAdapter.ViewHolder> {

    // Provide a direct reference to each of the views within a data item
    // Used to cache the views within the item layout for fast access
    public static class ViewHolder extends RecyclerView.ViewHolder {
        // Your holder should contain a member variable
        // for any view that will be set as you render a row
        public TextView nameTextView;
        public Button messageButton;

        // We also create a constructor that accepts the entire item row
        // and does the view lookups to find each subview
        public ViewHolder(View itemView) {
            // Stores the itemView in a public final member variable that can be used
            // to access the context from any ViewHolder instance.
            super(itemView);

            nameTextView = (TextView) itemView.findViewById(R.id.contact_name);
            messageButton = (Button) itemView.findViewById(R.id.message_button);
        }
    }
}
```

Now that we've defined the basic adapter and `ViewHolder`, we need to begin filling in our adapter. First, let's store a member variable for the list of contacts and pass the list in through our constructor:

```java
public class ContactsAdapter extends 
    RecyclerView.Adapter<ContactsAdapter.ViewHolder> {

    // ... view holder defined above...

    // Store a member variable for the contacts
    private List<Contact> mContacts;

    // Pass in the contact array into the constructor
    public ContactsAdapter(List<Contact> contacts) {
        mContacts = contacts;
    }
}
```

Every adapter has three primary methods: 

- `onCreateViewHolder` to inflate the item layout and create the holder, 
- `onBindViewHolder` to set the view attributes based on the data and 
- `getItemCount` to determine the number of items. 
 
We need to implement all three to finish the adapter:

```java
public class ContactsAdapter extends 
    RecyclerView.Adapter<ContactsAdapter.ViewHolder> {

    // ... constructor and member variables

    // Usually involves inflating a layout from XML and returning the holder
    @Override
    public ContactsAdapter.ViewHolder onCreateViewHolder(ViewGroup parent, int viewType) {
        Context context = parent.getContext();
        LayoutInflater inflater = LayoutInflater.from(context);

        // Inflate the custom layout
        View contactView = inflater.inflate(R.layout.item_contact, parent, false);

        // Return a new holder instance
        ViewHolder viewHolder = new ViewHolder(contactView);
        return viewHolder;
    }
    
    // Involves populating data into the item through holder
    @Override
    public void onBindViewHolder(ContactsAdapter.ViewHolder viewHolder, int position) {
        // Get the data model based on position
        Contact contact = mContacts.get(position);

        // Set item views based on the data model
        TextView textView = viewHolder.nameTextView;
        textView.setText(contact.getName());

        Button button = viewHolder.messageButton;

        if (contact.isOnline()) {
            button.setText("Message");
            button.setEnabled(true);
        }
        else {
            button.setText("Offline");
            button.setEnabled(false);
        }

    }

    // Return the total count of items
    @Override
    public int getItemCount() {
        return mContacts.size();
    }
}
```

With the adapter completed, all that is remaining is to bind the data from the adapter into the RecyclerView.

#### Binding the Adapter to the RecyclerView

In our activity, we will populate a set of sample users which should be displayed in the RecyclerView.

```java
public class UserListActivity extends AppCompatActivity {

     ArrayList<Contact> contacts;

     @Override
     protected void onCreate(Bundle savedInstanceState) {
         // ...
         // Lookup the recyclerview in activity layout
         RecyclerView rvContacts = (RecyclerView) findViewById(R.id.rvContacts);

         // Initialize contacts
         contacts = Contact.createContactsList(20);
         // Create adapter passing in the sample user data
         ContactsAdapter adapter = new ContactsAdapter(contacts);
         // Attach the adapter to the recyclerview to populate items
         rvContacts.setAdapter(adapter);
         // Set layout manager to position the items
         rvContacts.setLayoutManager(new LinearLayoutManager(this));
         // That's all!
     }
}
```

Finally, compile and run the app and you should see something like the screenshot below. If you create enough items and scroll through the list, the views will be recycled and far smoother by default than the ListView widget:

![](http://i.imgur.com/vbIL5HA.gif)

#### Notifying the Adapter

Unlike `ListView`, there is no way to add or remove items directly through the RecyclerView adapter. You need to make changes to the data source directly and notify the adapter of any changes. Also, whenever adding or removing elements, always make changes to the existing list. For instance, reinitializing the list of Contacts such as the following will not affect the adapter, since it has a memory reference to the old list:

```java
// do not reinitialize an existing reference used by an adapter
contacts = Contact.createContactsList(5);
```

Instead, you need to act directly on the existing reference:

```java
// add to the existing list
contacts.addAll(Contact.createContactsList(5));
```

There are many method available to use when notifying the adapter of different changes:

| Method | Description |
| ------ | ----------- |
| notifyItemChanged(int pos) | Notify that item at position has changed. |
| notifyItemInserted(int pos) | Notify that item reflected at position has been newly inserted. |
| notifyItemRemoved(int pos) | Notify that items previously located at position has been removed from the data set. |
| notifyDataSetChanged() | Notify that the dataset has changed. Use only as last resort. |

We can use these from the activity or fragment:

```java
// Add a new contact
contacts.add(0, new Contact("Barney", true));
// Notify the adapter that an item was inserted at position 0
adapter.notifyItemInserted(0);
```

Every time we want to add or remove items from the RecyclerView, we will need to explicitly inform to the adapter of the event. Unlike the ListView adapter, a RecyclerView adapter should not rely on notifyDataSetChanged() since the more granular actions should be used. See the [API documentation](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.Adapter.html) 
for more details.

Also, if you are intending to update an existing list, make sure to get the current count of items before making any changes. For instance, a getItemCount() on the adapter should be called to record the first index that will be changed.

```java
// record this value before making any changes to the existing list
int curSize = adapter.getItemCount();

// replace this line with wherever you get new records
ArrayList<Contact> newItems = Contact.createContactsList(20); 

// update the existing list
contacts.addAll(newItems);
// curSize should represent the first element that got added
// newItems.size() represents the itemCount
adapter.notifyItemRangeInserted(curSize, newItems.size());
```

#### Scrolling to New Items

If we are inserting elements to the front of the list and wish to maintain the position at the top, we can set the scroll position to the 1st element:

```java
adapter.notifyItemInserted(0);  
rvContacts.scrollToPosition(0);   // index 0 position
```

If we are adding items to the end and wish to scroll to the bottom as items are added, we can notify the adapter that an additional element has been added and can call smoothScrollToPosition() on the RecyclerView:

```java
adapter.notifyItemInserted(contacts.size() - 1);  // contacts.size() - 1 is the last element position
rvContacts.scrollToPosition(mAdapter.getItemCount() - 1); // update based on adapter 
```

#### Implementing Endless Scrolling

To implement fetching more data and appending to the end of the list as the user scrolls towards the bottom, use the `addOnScrollListener()` from the RecyclerView and add an onLoadMore method leveraging the `EndlessScrollViewScrollListener` document in the guide.

[To be continued...]

# 链接

[Using the RecyclerView](https://guides.codepath.com/android/using-the-recyclerview)
