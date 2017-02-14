We are very pleased to announce the immediate availability of Yii Framework version 1.1.9. In this release, we've included nearly 60 enhancements and bug fixes.

For the complete list of changes in this release, please see the [change log](http://www.yiiframework.com/files/CHANGELOG-1.1.9.txt) and [important feature additions](http://www.yiiframework.com/doc/guide/changes). And if you plan to upgrade from an older version to 1.1.9, refer to the [upgrade instructions](http://www.yiiframework.com/files/UPGRADE-1.1.9.txt).

In the following page, we briefly introduce some of the changes in this release.

## More convenient way to define through AR relation

Active Record though option was introduced in 1.1.7 but syntax wasn't convenient so we decided to make it more definitive. Current syntax is the following:

`'comments'=>array(self::HAS_MANY,'Comment',array('key1'=>'key2'),'through'=>'posts'),`

In the above array('key1'=>'key2'):

* key1 is a key defined in relation specified in through (posts is this case).
* key2 is a key defined in a model relation points to (Comment in this case).

through can be used with both HAS_ONE and HAS_MANY relations.

For more details and examples refer to [Relational Query with through](http://www.yiiframework.com/doc/guide/1.1/en/database.arr#relational-query-with-through).

## Scope support for Model::relations()

You can now use scopes when defining AR model relations such as

`'recentApprovedComments'=>array(self::BELONGS_TO, 'Post', 'post_id', 
    'scopes' => array('approved', 'recent')),`

In case of a single scope you can specify a single scope with a string instead of array.

## Ability to join AR models on a specific keys

It is now possible to build AR relations on PK->FK specified instead of relying on the schema defaults so you can specify AR relation like the following declaration for Day model:

`'jobs'=>array(self::HAS_MANY, 'Job', array('date' => 'target_date')),`

In the case above Day can have multiple Jobs but these aren't related usual way. We've specified relation key in form of array('fk'=>'pk'). That means it will generate SQL like

`SELECT * FROM day t
JOIN job ON t.date = job.target_date`

## Ability to override core classes using Yii::$classMap

There was an ability to [pre-import classes](http://www.yiiframework.com/doc/guide/1.1/en/basics.namespace#using-class-map) and use them without explicitly importing or including since 1.1.5. Now using the same technique you can override [Yii core classes](http://code.google.com/p/yii/source/browse/trunk/framework/YiiBase.php#632) with your own ones.