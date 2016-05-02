# Cannot add header view to list — setAdapter has already been called

### Question

I have one edittext field and one "search" button. When I click on search, I have to display a list view with data 
corresponding to the values entered in the edittext. I have added a header to my list using addHeader(). When I do 
search first time, I am able to display data in List successfully. But when I do search again, I am getting the below error.

```java
FATAL EXCEPTION: main
java.lang.IllegalStateException: Cannot add header view to list -- setAdapter has already been called.
at android.widget.ListView.addHeaderView(ListView.java:261)
at android.widget.ListView.addHeaderView(ListView.java:284)
```

I have assigned header to my list before setting the adapter.

Below is my code:

```java
myList = (ListView) findViewById(R.id.searchResultsList);
View header = View.inflate(this, R.layout.search_results_header, null);
myList.addHeaderView(header, null, false);
dataAdapter = new MyCustomAdapter(this, R.layout.results_list_item, searchedResults);
myList.setAdapter(dataAdapter);
```

Where I am doing wrong?

### Answer

On android 2.3, add header after setAdapter (even if you have added early, then removed) will throw an exception. 

To hide or show a header dynamically, use `setVisibility()`. How? You can see 
[Hiding header views](http://pivotallabs.com/android-tidbits-6-22-2011-hiding-header-views).

# 链接

[Cannot add header view to list — setAdapter has already been called](http://stackoverflow.com/questions/19583961/cannot-add-header-view-to-list-setadapter-has-already-been-called)
