A guide to Yii grids, lists and data providers
==============================================

Grids, lists and data providers are good addition to Yii core. These were originally
a part of zii, official extension library that was later merged with the framework
itself.

Typically, you would take the following steps when working with components such as grids, lists and data providers.

  1.  Supply data (in the form of “ar model”, “arrays” or “sql result set”) to the a data provider (such as `CActiveDataProvider`, `CArrayDataProvider` and `CSqlDataProvider`) .
  2.  Use a widget component (such as `CListView` or `CGridView` ) and configure it to display the data.
  3.  Customize the widget to reflect the presentational style that you are after. 
 
In this guide all these will be covered in detail.

~~~
data-provider-intro
data-providers
list-intro
list-xxx
grid-intro
grid-columns
grid-filtering
~~~