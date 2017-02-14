We are very pleased to announce the immediate availability of Yii Framework version 1.1.7. In this release, we included more than 90 new features, enhancements and bug fixes.

For the complete list of changes in this release, please see the [change log](http://www.yiiframework.com/files/CHANGELOG-1.1.7.txt) and [important feature additions](http://www.yiiframework.com/doc/guide/changes). And if you plan to upgrade from an older version to 1.1.7, refer to the [upgrade instructions](http://www.yiiframework.com/files/UPGRADE-1.1.7.txt).

In the following, we briefly introduce some of the most exciting new features in this release. Thank you for your support!

## RESTful URL Support

To let [CUrlManager](http://www.yiiframework.com/doc/api/CUrlManager) to recognize specific HTTP verbs and respond accordingly, one can specify the verb option in the URL rules in the application configuration. For example, with the following rules, a GET request for post/123 will be handled by post/view action, while a PUT or POST request for post/123 will be handled by post/update action.

```
return array(
    'components'=>array(
        'urlManager'=>array(
            'urlFormat'=>'path',
            'rules'=>array(
                array('<controller>/view', 'pattern'=>'<controller:\w>/<id:\d+>', 'verb'=>'GET'),
                array('<controller>/update', 'pattern'=>'<controller:\w>/<id:\d+>', 'verb'=>'PUT, POST'),
 
            ),
        ),
    ),
);
```
Additionally, one can use [CHttpRequest::getPut()](http://www.yiiframework.com/doc/api/CHttpRequest#getPut) and [CHttpRequest::getDelete()](http://www.yiiframework.com/doc/api/CHttpRequest#getDelete) to retrieve the data submitted via PUT and DELETE.

## Query Caching

Built on top of data caching, query caching stores the result of a DB query in cache and may thus save the DB query execution time if the same query is requested in future, as the result can be directly served from the cache.

Query caching can be used at both DAO and AR levels. The following code shows some usage examples:

```
// cache the results of $sql for 1000 seconds or until tbl_post is updated
$sql = 'SELECT * FROM tbl_post LIMIT 20';
$dependency = new CDbCacheDependency('SELECT MAX(update_time) FROM tbl_post');
$rows = Yii::app()->db->cache(1000, $dependency)->createCommand($sql)->queryAll();
 
// same as the above but with AR
$posts = Post::model()->cache(1000, $dependency)->findAll();
// query caching with relational AR
$posts = Post::model()->cache(1000, $dependency)->with('author')->findAll();
```
## Parameter Binding for Class-based Actions

In version 1.1.4, we added support for automatically populating method-based action parameters. In this version, we extended this support to class-based actions. The following is an example:

```
class UpdateAction extends CAction
{
    public function run($id)
    {
        // $id will be automatically populated with $_GET['id']
    }
}
```
## Seamless Client-side Validation

[CActiveForm](http://www.yiiframework.com/doc/api/CActiveForm) is a powerful widget that makes creating form and performing data validation very easy. Previously, [CActiveForm](http://www.yiiframework.com/doc/api/CActiveForm) supports server-side validation and AJAX-based validation. In this version, we enhanced [CActiveForm](http://www.yiiframework.com/doc/api/CActiveForm) further with client-side validation.

Compared with server-side and AJAX-based validations, the new validation is performed entirely on the client side. Therefore, it can give more prompt response to user inputs and will not bring any extra workload on the server.

The best thing about this client-side validation is that it does not require any extra coding. The validation is done based on the validation rules declared in the model class, and the result is consistent with that done on the server side. All these make our code more DRY.

To use client-side validation, we simply need to set [CActiveForm::enableClientValidation](http://www.yiiframework.com/doc/api/CActiveForm#enableClientValidation) to be true, like the following:

```
<?php $form=$this->beginWidget('CActiveForm', array(
    'enableClientValidation'=>true,
)); ?>
 
    <div class="row">
        <?php echo $form->labelEx($model,'username'); ?>
        <?php echo $form->textField($model,'username'); ?>
        <?php echo $form->error($model,'username'); ?>
    </div>
 
    <div class="row">
        <?php echo $form->labelEx($model,'password'); ?>
        <?php echo $form->passwordField($model,'password'); ?>
        <?php echo $form->error($model,'password'); ?>
    </div>
 
    <div class="row buttons">
        <?php echo CHtml::submitButton('Login'); ?>
    </div>
 
<?php $this->endWidget(); ?>
```
## Passing Parameters to Relational Named Scopes

It's now possible to pass parameters for relational named scopes. For example, if you have scope named rated in the Post that accepts minimum rating of post, you can use it from User the following way:

```
$users=User::model()->findAll(array(
    'with'=>array(
        'posts'=>array(
            'scopes'=>array(
                'rated'=>5,
            ),
        ),
    ),
));
```
## Using 'through' with HAS_MANY and HAS_ONE

Active Record got through support that allows to build MANY_MANY-like relations with much more flexibility allowing getting and using data from the binding in-the-middle table.

For example, it allows getting all comments for all users of a particular group.

You can learn more by reading the corresponding guide section.

## Using Transactions in DB Migration

When performing DB migration, we may encounter situations where a part of a migration fails and we want to roll back the whole migration. The solution to this problem is to enclose the whole migration code within a DB transaction. While we can explicitly write this code, the new transaction support for DB migration makes the whole task a lot easier.

Instead of implementing [CDbMigration::up()](http://www.yiiframework.com/doc/api/CDbMigration#up) and [CDbMigration::down()](http://www.yiiframework.com/doc/api/CDbMigration#down), we can put our code in [CDbMigration::safeUp()](http://www.yiiframework.com/doc/api/CDbMigration#safeUp) and [CDbMigration::safeDown()](http://www.yiiframework.com/doc/api/CDbMigration#safeDown). By doing so, our code will be protected by DB transaction. In case any DB operation fails in the middle, the whole migration will be rolled back. The following is an example:

```
class m101129_185401_create_news_table extends CDbMigration
{
    public function safeUp()
    {
        $this->createTable('tbl_news', array(
            'id' => 'pk',
            'title' => 'string NOT NULL',
            'content' => 'text',
        ));
    }
 
    public function safeDown()
    {
        $this->dropTable('tbl_news');
    }
}
```
Note that in order to use this feature, you DBMS must support transactions.

## Registering and Using Custom Script Packages

[CClientScript](http://www.yiiframework.com/doc/api/CClientScript) now allows developers to register and use their own script packages. Previously this feature was only available for core framework packages.

A script package may include CSS files, JavaScripts, image files, etc., which need to be accessed by end users. A script package may depend on other packages. That is, if a package is registered to be rendered in a page, all its dependent packages must also be registered and rendered.

Previously, [CClientScript](http://www.yiiframework.com/doc/api/CClientScript) only supported using core framework script packages (via [CClientScript::registerCoreScript](http://www.yiiframework.com/doc/api/CClientScript#registerCoreScript)). In this version, we extended this feature and made it available to custom script packages.

To use this feature, one first needs to register the custom packages by configuring the [CClientScript::packages](http://www.yiiframework.com/doc/api/CClientScript#packages) property. One then calls CClientScript::registerPackage() to register a package when needed.