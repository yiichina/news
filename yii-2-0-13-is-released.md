We are very pleased to announce the release of Yii Framework version 2.0.13. Please refer to the instructions at http://www.yiiframework.com/download/ to install or upgrade to this version.

Version 2.0.13 is a minor release of Yii 2.0 which contains more than 90 enhancements and bug fixes.

There are minor changes that may affect your existing applications and also an important change about compatibility with PHP 7.2, so make sure to check the UPGRADE.md file.

Huge thanks to the Yii community for all the valuable contributions!

You may follow the development progress of Yii 2 by starring or watching Yii 2.0 GitHub Project. You may also follow Yii Twitter feeds or join Yii Facebook group to connect with other Yii developers. There is also a forum thread about this news announcement.

Also since there is Yii 2.1 in development now, make sure you have a version constraint in your composer.json, that does not allow it to be installed automatically on update, so when next major version of Yii is released, your project won't break by itself. A version constraint that does not include 2.1 is for example ~2.0.13, make sure you do not have >= or * in version constraints in composer.json.

Below we summarize some of the most important features/fixes included in this release. A complete list of changes can be found in the CHANGELOG.

## Application templates

Both "basic" and "advanced" application templates are now using asset-packagist.org and aren't relying on Composer asset plugin.

Advanced application Vagrant box got XDdebug installed by default. Also it's now simpler to install since all the required plugins are installed automatically.

Basic template got Alert widget similar to existing widget from advanced application.

## Logging, debugging and error handling

Logging, debugging and error handling are daily routines for every developer so these need to work flawlessly and provide as useful info as possible. We are continuously improving these areas of the framework. In this version there are the following enhancements:

yii\web\DbSession now relies on error handler to display errors. As we have found out, it was common to forget to check for existence of objects when writing extra session data into database. Debugging it was very hard because errors displayed were about fail to write session or alike. Now, it properly displays the real cause.
Migrations were previously allowed to work with names of any size. Now, they are limited to 180 symbols and throw exceptions because too long migration names are likely to be a problem if you will try applying these to different DBMS.
Log messages got milliseconds.
More than that, Carsten Brandt enhanced exception page with three new buttons:

v7-koft0ekcldbeecrbt8zzytdw.png

## Data filters

Thanks to the excellent work of Paul Klimov, we have got data filters and their integration with REST API framework. Filters could be used to build flexible complex data provider conditions. They are very customizable and may fit both web and REST API applications.

Documentation is availale in the guide. See "Filtering Data Providers using Data Filters".

Concurrency handling

Proper concurrency handling is important for high traffic projects. In this release we have two important fixes about it. First, @kidol fixed possible race conditions in yii\mutex\FileMutex. Second, @dynasource did the same for asset publishing in case symlinks are enabled.

Dependency Injection container and its usage

There are multiple enhancements made to DI container and how it is used:

Variadic parameters support added.
Dependencies are automatically injected into migration constructor now if defined.
Container is now used to instantiate cookies in order to be able to set global defaults.
PHP 7.2 compatibility

Release candidates for PHP 7.2 are currently being published and its soon becoming a stable version. The next version of PHP 7 comes with some improvements that break backwards compatibility, so framework and application code need to be adjusted. Since this release, Yii 2 is fully compatible with PHP 7.2, so you can update your server environment.

Among smaller changes, there is one change that affects Yii compatibility heavily. Since PHP 7.2 there are new forbidden class names of which one is object. This was decided in a PHP RFC and means that you cannot have a class named object anymore without producing a fatal error. Yii has yii\base\Object which is the base class of all framework classes and also many classes in extensions and application code depend on it.

To be able to make Yii work on PHP 7.2 we needed to rename this class, so yii\base\Object is now deprecated in favor of yii\base\BaseObject. All Versions of Yii 2 before 2.0.13 are not compatible with PHP 7.2.

Please refer to the UPGRADE instructions on more of the technical details of adjusting your code to work with these changes.

Request

Request class got yii\web\Request::getOrigin() method that returns HTTP_ORIGIN of current CORS request.

There is now also a handy shortcut, yii\filters\AjaxFilter, that allows to limit access to controller action to AJAX requests only.

## Security

A non-critical issue with error handler not escaping error info in debug mode, CVE-2017-11516, was fixed.

Request got its security slightly enhanced by adding support for specifying trusted proxies. In the process we have disabled obtaining HTTP/HTTPS status via X-Forwarded-Proto by default.

## Console

Console got two interesting enhancements. First is yii\console\ExitCode class that defines a good number of constants that could be used to indicate different outcomes of console command execution:
```
if ($this->importData()) {
    return ExitCode::OK;
} else {
    ExitCode::SOFTWARE;
}
```
Another enhancement is an ability to render tables in console commands:
```
echo Table::widget([
    'headers' => ['test1', 'test2', 'test3'],
    'rows' => [
        ['col1', 'col2', 'col3'],
        ['col1', 'col2', ['col3-0', 'col3-1', 'col3-2']],
    ],
]);
```
will output
```
╔═══════╤═══════╤══════════╗
║ test1 │ test2 │ test3    ║
╟───────┼───────┼──────────╢
║ col1  │ col2  │ col3     ║
╟───────┼───────┼──────────╢
║ col1  │ col2  │ • col3-0 ║
║       │       │ • col3-1 ║
║       │       │ • col3-2 ║
╚═══════╧═══════╧══════════╝
```
## Database

Sergey Makinen implemented a solution for managing DBMS constraints:
```
$connection = Yii::$app->db;
 
$connection
    ->createCommand()
    ->addUnique('unique-username', 'user', 'username')
    ->execute();
 
$connection
    ->createCommand()
    ->dropUnique('unique-username', 'user')
    ->execute();
 
$connection
    ->createCommand()
    ->addCheck('check-price', 'order', 'price > 0')
    ->execute();
 
$connection
    ->createCommand()
    ->dropCheck('check-price', 'order')
    ->execute();
 
$connection
    ->createCommand()
    ->addDefaultValue('default-balance', 'account', 'balance', 0)
    ->execute();
 
$connection
    ->createCommand()
    ->dropDefaultValue('default-balance', 'account')
    ->execute();
```
It is also now possible to get info about various schema info:
```
/** @var Schema $schema */
$schema = $connection->getSchema();
$fks = $schema->getTableForeignKeys('user');
foreach ($fks as $fk) {
    echo $fk->onUpdate;
}
```
Check this commit to learn more about it.

## Interfaces

We have started to gradually introduce interfaces for some of our components. This time it is yii\caching\CacheInterface More to come in both 2.0 and 2.1.

## Migrations

New migrate/fresh command was added. It truncates database and re-applies all migrations anew.

There are also two new options in Migration class, $compact that makes output more compact and $maxSqlOutputLength that limits output of SQL to specified number of characters.

## Modules

Module service locator accessible as from its controllers as $this->module->get('myComponent') now falls back to its parent module service locator in case component is not found. Since the parent of all modules is an application, the component will be searched in the application container in case it is not found in the module itself and any of its parents.

This change aids developer in making modules more isolated. By using $this->module->get('myComponent') you automatically have an ability to redefine module dependencies right from the module config while having usual application components as defaults.

See "Accessing components from within modules" for example.

## Helpers

ArrayHelper got setValue() method. It writes a value into an associative array at the key path specified. If there is no such key path yet, it will be created recursively. If the key exists, it will be overwritten. Two syntaxes are possible:
```
ArrayHelper::setValue($array, 'key.in', ['arr' => 'val']);
```
and
```
ArrayHelper::setValue($array, ['key', 'in'], ['arr' => 'val']);
```
Another helper method that was added in this release is StringHelper::floatToString(). It is obtaining a string from a float value but, unlike casting float to string natively, it always uses dot as decimal separator.

## RBAC

yii\rbac\DbManager::checkAccess() stopped doing duplicate queries for user assignments so if you are using it, chances are that you will get performance improvements. Additionally, extra index was added to RBAC tables so queries themselves are faster.

## jQuery 3.0 compatibility

While in Yii 2.1 we are trying to make core framework JS-framework independent, 2.0 is tied to jQuery so we have ensured that both our JS and PHP code is compatible with jQuery 3.0.

## ActiveRecord and behaviors

A new method public static function instance($refresh = false); has been added to the yii\db\ActiveRecordInterface via a new yii\base\StaticInstanceInterface. The purpose is to provide static instances to classes, which can be used to obtain class meta information that cannot be expressed in static methods. For example: adjustments made by DI or behaviors reveal only at object level, but might be needed at class (static) level as well.

yii\behaviors\AttributesBehavior was added. It assigns values specified to one or multiple attributes of an AR object when certain events happen. Unlike yii\behaviors\AttributeBehavior it works with multiple attributes at once. Both AttributeBehavior and AttributesBehavior got preserveNonEmptyValues setting that, when set to true, allow you to overwrite the value only if it is empty.

yii\behaviors\SluggableBehavior got skipOnEmpty option that, when set to true, allows behaviour not to generate a new slug if attribute is null or an empty string.

## Assets

Asset hashing now takes asset symlinking into account to improve cache busting. That would improve a situation when you have worked with copying directories and then switching to using symlinks. Previously you need to delete assets directory contents, with 2.0.13 it is fine without doing so.

Asset compression and combining command now takes into account that some libraries are not including trailing ; thus making some combinations of scripts fail. Now, it is adding extra ; where needed.

## CSRF and cache

There is now yii\web\View::registerCsrfMetaTags() method that registers CSRF tags dynamically ensuring that caching does not interfere.

## New formatter methods

Formatter got new methods:

asWeight() formats a number as weight such as "12 kilograms".
asShortWeight() formats a number as short weight such as "12 kg".
asLength() formats a number as length such as "12 meters".
asShortLength() formats a number as short length such as "12 m".
