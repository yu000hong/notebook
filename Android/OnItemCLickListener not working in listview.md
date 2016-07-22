# OnItemCLickListener not working in listview

The problem is that your layouts contain either `focusable` or `clickable` items. 

If a view contains either `focusable` or `clickable` item the `OnItemCLickListener` won't be called.

Click [here](http://stackoverflow.com/questions/2098558/listview-with-clickable-editable-widget) for more information.

All you need do is set `android:focusable` and `android:clickable` to `false`.

