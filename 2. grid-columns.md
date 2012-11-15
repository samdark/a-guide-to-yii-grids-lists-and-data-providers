Grid columns
============

Yii grid consists of a number of columns. Depending on column type and settings
these are able to present data differently.

These are defined in the `columns` part of `CGridView` config like the following:

~~~
$this->widget('zii.widgets.grid.CGridView', array(
    'dataProvider'=>$dataProvider,
    'columns'=>array(
    	// A simple column defined by the data contained in $dataProvider.
    	// Data from model's column1 will be used.
    	'column1',
    	// More complex one.
        array(
        	'class' => 'CDataColumn', // can be omitted, default
            'name'=>'column1',
            'value'=>function($data,$row){
            	return $data->name;
            },
            'type'=>'raw',
        ),
    ),
));
~~~

> Note: If `columns` part of config isn't specified, Yii tries to show all possible
> data provider model columns.

Column classes
--------------

There are several column classes bundled with Yii:

- `CButtonColumn`.
- `CCheckBoxColumn`.
- `CDataColumn`.
- `CLinkColumn`.

All these are extended from base `CGridColumn` that represents the specification
for rendering the cells in a particular grid view column.

In a column, there is:

- one header cell.
- multiple data cells.
- an optional footer cell.

Child classes may override `renderHeaderCellContent`, `renderDataCellContent` and
`renderFooterCellContent` and `renderFilterCellContent` to customize how these
cells are rendered.

Let's review what we already have out of the box.

### CButtonColumn

This one represents a grid column that renders one or several buttons. By default
it's used as simple as:

~~~
$this->widget('zii.widgets.grid.CGridView', array(
    'dataProvider'=>$dataProvider,
    'columns'=>array(
    	// …
        array(
        	'class' => 'CButtonColumn',
        ),
    ),
));
~~~

The code above will display three buttons, "view", "update" and "delete", which
triggers the corresponding actions on the model of the row. By default actions for
these are expected to be in the same controller as rendered the grid.

By configuring `buttons` and `template` properties, the column can display other
buttons and customize the display order of the buttons.

### CCheckBoxColumn

This one represents a grid view column of checkboxes. `CCheckBoxColumn`
supports no selection (read-only), single selection and multiple selection.
The mode is determined according to `selectableRows`. When in multiple selection mode,
the header cell will display an additional checkbox, clicking on which will check
or uncheck all of the checkboxes in the data cells.

Additionally selecting a checkbox can select a grid view row
(depending on `CGridView::selectableRows` value) if `selectableRows` is `null` (default).

By default, the checkboxes rendered in data cells will have the values that are
the same as the key values of the data model. One may change this by setting
either `name` or `value`.

### CDataColumn

Represents a grid view column that is associated with a data attribute or expression.
This column class is used by default if no other class is specified:

~~~
$this->widget('zii.widgets.grid.CGridView', array(
    'dataProvider'=>$dataProvider,
    'columns'=>array(
        array(
            'name'=>'column1',
            'value'=>function($data,$row){
            	return $data->name;
            },
            'type'=>'raw',
        ),
    ),
));
~~~

Either `name` or `value` should be specified. The former specifies a data
attribute name, while the latter a PHP expression whose value should
be rendered instead.

The `value` itself is a PHP expression or callback that will be evaluated for every data cell
and whose result will be rendered as the content of the data cells. In this expression,
the variable `$row` holds the row number (zero-based) and `$data` contains the data
model for the row. `$this` corresponds to the column object itself.

~~~
$this->widget('zii.widgets.grid.CGridView', array(
    'dataProvider'=>$dataProvider,
    'columns'=>array(
        array(
            'name'=>'column1',
            'value'=>'$data->name',
        ),
    ),
));
~~~

In case of PHP 5.3 and newer you can use anonymous function instead of a string:

~~~
$this->widget('zii.widgets.grid.CGridView', array(
    'dataProvider'=>$dataProvider,
    'columns'=>array(
        array(
            'name'=>'column1',
            'value'=>function($data,$row){
            	return $data->name;
            },
        ),
    ),
));
~~~

In order to get any outside variable into `value` anonymous function we can rely
on PHP's `use`:

~~~
$greeting = "Hello, ";
$this->widget('zii.widgets.grid.CGridView', array(
    'dataProvider'=>$dataProvider,
    'columns'=>array(
        array(
            'name'=>'column1',
            'value'=>function($data,$row) use ($greeting){
            	return $greeting.$data->name;
            },
        ),
    ),
));
~~~

The property `sortable` determines whether the grid view can be sorted according
to this column. Note that the `name` should always be set if the column needs to be
sortable. The `name` value will be used by `CSort` to render a clickable link in
the header cell to trigger the sorting. You can specify `value` as well if you need
to display some custom value for this column and still have it as sortable.

There are multiple formatting types this column class can use. In order
to switch between these you can set `type` property to the one of the following:

- raw — the attribute value will be rendered as is so it's especially useful in
  case of complex formatting. Be careful with this one and make sure output is
  properly escaped.
- text (default) — the attribute value will be HTML-encoded.
- ntext — formats the value as a HTML-encoded plain text and converts newlines with HTML `<br/>` tags.
- html — the attribute value will be sanitized using HTMLPurifier. Note that
  it's not that good for performance so don't overuse it.
- date — formats the value as a data using PHP's `date` and `dateFormat`
  property as format string. Default format is `Y/m/d`.
- time — formats the value as a time using PHP's `date` and `timeFormat`
  property as format string. Default format is `h:i:s A`.
- datetime — formats the value as a date and time using PHP's `date` and `datetimeFormat`
  property as format string. Default format is `Y/m/d h:i:s A`.
- boolean — value is formatted as boolean. By default that means a simple `Yes` or
  `No` text. These could be overridden using `booleanFormat` property.
- number — formats the value as a number using PHP `number_format()` function.
- email — formats the value as a mailto link using `CHtml::mailto`.
- image — formats the value as an image tag using `CHtml::image`.
- url — formats the value as a hyperlink.

These are all methods of [CFormatter](http://www.yiiframework.com/doc/api/1.1/CFormatter)
or an application component with id `formatter` if you set one through application
configuration. That means you can change formatting options for such formats as
`date` or `time` application wide.

- TODO: `filter`

### CLinkColumn

Represents a grid view column that renders a hyperlink in each of its data cells.

The `label` and `url` properties determine how each hyperlink will be rendered.
The `labelExpression`, `urlExpression` properties may be used instead if they
are available. In addition, if `imageUrl` is set, an image link will be rendered.

Implementing our own column class
---------------------------------

