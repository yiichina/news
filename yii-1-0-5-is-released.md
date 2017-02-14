We are proud to announce the release of Yii Framework v1.0.5!

In this release, we included nearly thirty feature enhancements and a dozen of bug fixes. Some of the major feature enhancements are summarized as follows.

We made several significant improvements to the ActiveRecord (AR) feature in Yii. We implemented named scopes (thanks to Ruby on Rails for its inspiration) in AR, which allows us to write snappy and chainable queries like Post::model()->published()->recently()->findAll(). We further improved the lazy loading feature of AR by supporting on-the-fly query options. We also added support for using Oracle with AR, thanks to our community member Ricardo for his generous contribution.

We enhanced the URL management functionality. In particular, sub-patterns can now be used in URL routes. This makes the two-way URL management (URL routing and creation) in Yii even more powerful.

While we are adding many new features, performance remains to be a central piece in Yii. In this release, we optimized the database queries used in role-based access control (RBAC). We also added support for caching the result of RBAC.

A list of all issues resolved in this release can be found in the [change log](http://www.yiiframework.com/files/CHANGELOG-1.0.5.txt).

Cheers!

The Yii Developer Team