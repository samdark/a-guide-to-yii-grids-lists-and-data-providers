Introduction to grids
=====================

In Yii grid widget is extremely useful if you need to quickly build admin section
of the system. It takes data from data provider and renders each row using a set of
columns presenting data in a form of a table.

Each row of the table represents the data of a single data item, and a column
usually represents an attribute of the item (some columns may correspond to
complex expression of attributes or static text).

`CGridView` supports both sorting and pagination of the data items. The sorting
and pagination can be done in AJAX mode or normal page request. A benefit of
using `CGridView` is that when the user browser disables JavaScript, the sorting
and pagination automatically degenerate to normal page requests and are still
functioning as expected.

`CGridView` should be used together with a data provider, preferrably a
`CActiveDataProvider`.

The minimal code needed to use CGridView is as follows:

~~~
$dataProvider=new CActiveDataProvider('Post');

$this->widget('zii.widgets.grid.CGridView', array(
    'dataProvider'=>$dataProvider,
));
~~~

The above code first creates a data provider for the Post ActiveRecord class.
It then uses `CGridView` to display every attribute in every Post instance.
The displayed table is equiped with sorting and pagination functionality.

See [grid-columns] for details about how to configure different columns.