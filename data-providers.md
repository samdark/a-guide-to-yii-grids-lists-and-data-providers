Data providers
==============

Data provider abstracts getting data by providing `IDataProvider` interface and
handles pagination and sorting.

It can be used by grids, lists and all other classes extended from
`CBaseListView` as well as in custom code.

In Yii there three default data providers: `CActiveDataProvider`, `CArrayDataProvider`
and `CSqlDataProvider`.

Active data provider
--------------------

`CActiveDataProvider` implements a data provider based on ActiveRecord.

Array data provider
-------------------

`CArrayDataProvider` implements a data provider based on a raw data array.

SQL data provider
-----------------

`CSqlDataProvider` implements a data provider based on a plain SQL statement.

Implementing your own custom data provider
------------------------------------------

In order to implement your own data provider …