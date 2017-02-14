We are proud to announce the release of Yii Framework v1.0.9!

In this release, we fixed about a dozen of bugs and included more than twenty minor feature enhancements. In particular, we improved the performance of executing relational AR queries in a lazy way by avoiding unnecessary SQL JOINs. For a list of all enhancements and issues resolved in this release, please see [change logs](http://www.yiiframework.com/files/CHANGELOG-1.0.9.txt).

Upgrading to v1.0.9 should be safe except that you should pay attention to lazy loading related objects of an AR object. With the new change, the related table will no longer join with the primary table (since it already knows the primary key of it), you should avoid referencing the primary in the query options of the corresponding relation.