We are very pleased to announce the release candidate of Yii Framework version 1.1.14. You can download it at [GitHub](https://github.com/yiisoft/yii/releases/tag/1.1.14-rc).

In this release, we fixed more than 80 bugs and introduced more than 60 minor enhancements and features. We added `CPasswordHelper` that provides secure and up to date way to store and verify password hashes; We added `CRedisCache` to support using Redis for caching purpose; and Yii can now be installed as a [Composer package](https://packagist.org/packages/yiisoft/yii). For the complete list of changes in this release, please see [the change log](https://raw.github.com/yiisoft/yii/1.1.14-rc/CHANGELOG).

We have received significant contributions to this release from our community users (e.g. creocoder, tom--, paystey, Ragazzo, antoncpu, Yiivgeny) to this release. We hereby thank their effort in making this release available.

Since this is a release candidate (RC), please do not use it for production. We will make a stable release of 1.1.14 in the next few weeks. Before that happens, **please help us test this RC** and [report to us any issues](https://github.com/yiisoft/yii/issues) **you find**. Thank you.

Below we summarize the main enhancements introduced in this release:

## New helper class CPasswordHelper

`CPasswordHelper` that provides secure and up to date way to store and verify password hashes. Usage is extremely easy:

```
// generate hash that will be stored to DB, $password is from accout creation form
$hash = CPasswordHelper::hashPassword($password);
 
// $hash is what we've saved to DB, $password is from login form
if (CPasswordHelper::verifyPassword($password, $hash)
  // password is good
else
  // password is bad
```

## New method 
## CDbCommandBuilder::createMultipleInsertCommand()

There's now `CDbCommandBuilder::createMultipleInsertCommand()` to support insertion of multiple records in a single query:

```php
$builder=Yii::app()->db->schema->commandBuilder;
$command=$builder->createMultipleInsertCommand('tbl_post', array(
  array('title' => 'record 1', 'text' => 'text1'),
  array('title' => 'record 2', 'text' => 'text2'),
));
$command->execute();
```

## New cache component CRedisCache

You can now configure application to use Redis as cache backend via `protected/config/main.php`, components section:

```php
array(
  // ...
  'components'=>array(
    'cache'=>array(
      'class'=>'CRedisCache',
      'hostname'=>'localhost',
      'port'=>6379,
      'database'=>0,
    ),
  ),
)
```

## Installing Yii as a [Composer package](https://packagist.org/packages/yiisoft/yii):

Yii is now can be installed using [Composer](https://packagist.org/packages/yiisoft/yii) by adding the following to composer.json.

`"yiisoft/yii": "dev-master"`

## Logging out a user after specified time

If you need a user to be logged out after specified time was passed regardless of his activity you now can use:

```php
// ...
  'components'=>array(
    'user'=>array(
      // ...
      'absoluteAuthTimeout' => 60*60*24;
    ),
  ),
)
```

## RANGE support for file downloading

When using `CHttpRequest::sendFile` Yii is now responding to RANGE requests. That means faster downloads when client is using multiple threads to download multiple parts of the file.

## Using through option with BELONGS_TO

"through" option can now be used with `CActiveRecord::BELONGS_TO.` For details [check the guide](http://www.yiiframework.com/doc/guide/1.1/en/database.arr#relational-query-with-through).

## Selective logging of global variables

`CLogFilter::$logVars` can now be array of arrays intended for designating particular items of the `$GLOBALS`. For example,

```php
'components'=>array(
        'log'=>array(
            'class'=>'CLogRouter',
            'routes'=>array(
                array(
                    'class'=>'system.logging.CWebLogRoute',
                    'filter'=>array(
                        'class'=>'system.logging.CLogFilter',
                        'logVars'=>array(
                            '_COOKIE', // old syntax, < 1.1.14
                            array('_SERVER','REMOTE_ADDR'), // new syntax, >= 1.1.14
                            array('_SERVER','HTTP_USER_AGENT'), // new syntax, >= 1.1.14
                        ),
                    ),
                ),
            ),
        ),
    ),
```
The configuration shown above would log this:

```php
$_COOKIE=array (
  '__utma' => '111872281.473431406.1366046648.1366046648.1366046648.1',
  '__utmz' => '111872281.1366046648.1.1.utmcsr=(direct)|utmccn=(direct)|utmcmd=(none)',
  'cartVisible' => '1',
  '96ab4aec0c2977c9b793d4ba009eb3ce' => '...',
  'PHPSESSID' => 'vb8pk7obs3q2lc7bl8ield7si7',
)
 
$_SERVER.REMOTE_ADDR='::1'
 
$_SERVER.HTTP_USER_AGENT='Mozilla/5.0 (Windows NT 6.1; WOW64; rv:22.0) Gecko/20100101 Firefox/22.0'
```

## New formatter CLocalizedFormatter

The new formatter class `CLocalizedFormatter` allows formatting values according to current locale. For example,

```php
'components'=>array(
    'format'=>array(
        'class'=>'system.utils.CLocalizedFormatter',
        'locale'=>'en_US',
    ),
    'germanFormat'=>array(
        'class'=>'system.utils.CLocalizedFormatter',
        'locale'=>'de_DE',
    ),
    'russianFormat'=>array(
        'class'=>'system.utils.CLocalizedFormatter',
        'locale'=>'ru_RU',
    ),
),

echo Yii::app()->format->formatDatetime(time()) . "\n";
echo Yii::app()->germanFormat->formatDatetime(time()) . "\n";
echo Yii::app()->russianFormat->formatDatetime(time()) . "\n";
 
// Expected result/output:
// Jul 7, 2013 9:34:56 PM
// 07.07.2013 21:34:56
// 07.07.2013, 21:34:56
```

## Generating attribute labels from column comments

Gii is now able to use table columns' comments as the attribute labels of a new generated model. For example, if a table is defined as follows,

```php
CREATE TABLE `tbl_user` (
  `id`  int(11) NOT NULL AUTO_INCREMENT,
  `first_name` varchar(100) NOT NULL COMMENT 'Имя:',
  `last_name` varchar(100) NOT NULL COMMENT 'Nachname:',
  `title` varchar(50) NOT NULL COMMENT 'Title (e.g. Mr., Mrs., etc.):',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=1;
```
The generated attributeLabels method would be (in case the option "Use Column Comments as Attribute Labels" in Gii is checked):

```php
/**
 * @return array customized attribute labels (name=>label)
 */
public function attributeLabels()
{
    return array(
        'id' => 'ID',
        'first_name' => 'Имя:',
        'last_name' => 'Nachname:',
        'title' => 'Title (e.g. Mr., Mrs., etc.):',
    );
}
```

## New methods in CSecurityManager

The following new methods are available in `CSecurityManager`:

* `CSecurityManager::generateSessionRandomBlock()`: generates random bytes based on the current user session.
* `CSecurityManager::generatePseudoRandomBlock()`: better and more reliable alternative to `mt_rand()`.
* `CSecurityManager::generateRandomBytes()`: generates random binary data. Cryptographical strong characteristic is changeable via second argument. Note, cryptographically strong random generation process is relatively slow.
* `CSecurityManager::generateRandomString()`: generates random string. Cryptographical strong characteristic is changeable via second argument. Note, cryptographically strong random generation process is relatively slow.
