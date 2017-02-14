We are very pleased to announce the immediate availability of Yii Framework version 1.1.11. In this release, we've included more than 100 enhancements and bug fixes.

This release is very special because it's the first release since [we've moved to github](http://www.yiiframework.com/news/53/yii-moved-to-github/) and a lot of work are contributed by [our awesome community](https://github.com/yiisoft/yii/graphs/contributors), including new features, bug fixes, unit tests, and of course, translations.

We want to thank all people who invested their time and brainpower in getting Yii better: [resurtm](https://github.com/resurtm), [DaSourcerer](https://github.com/DaSourcerer), [cebe](https://github.com/cebe), [suralc](https://github.com/suralc) and [many others](https://github.com/yiisoft/yii/graphs/contributors).

For the complete list of changes in this release, please see the [change log](http://www.yiiframework.com/files/CHANGELOG-1.1.11.txt) and [important feature additions](http://www.yiiframework.com/doc/guide/changes). And if you plan to upgrade from an older version to 1.1.11, please refer to the [upgrade instructions](http://www.yiiframework.com/files/UPGRADE-1.1.11.txt).

In the following page, we briefly introduce some of the changes in this release.

## HTML5 fields support in CHtml

We've added a group of new methods to CHtml:

* [CHtml::dateField()](http://www.yiiframework.com/doc/api/1.1/CHtml/#dateField-detail)
* [CHtml::rangeField()](http://www.yiiframework.com/doc/api/1.1/CHtml/#rangeField-detail)
* [CHtml::numberField()](http://www.yiiframework.com/doc/api/1.1/CHtml/#numberField-detail)
* [CHtml::emailField()](http://www.yiiframework.com/doc/api/1.1/CHtml/#emailField-detail)
* [CHtml::urlField()](http://www.yiiframework.com/doc/api/1.1/CHtml/#urlField-detail)
* [CHtml::activeDateField()](http://www.yiiframework.com/doc/api/1.1/CHtml/#activeDateField-detail)
* [CHtml::activeRangeField()](http://www.yiiframework.com/doc/api/1.1/CHtml/#activeRangeField-detail)
* [CHtml::activeNumberField()](http://www.yiiframework.com/doc/api/1.1/CHtml/#activeNumberField-detail)
* [CHtml::activeEmailField()](http://www.yiiframework.com/doc/api/1.1/CHtml/#activeEmailField-detail)
* [CHtml::activeUrlField()](http://www.yiiframework.com/doc/api/1.1/CHtml/#activeUrlField-detail)

Their usages are all similar to the following:

`echo CHtml::activeNumberField($model, 'fieldName');`

## CFormatter::formatSize()

This is a new method that allows you to get nicely formatted size units from size in bytes:

`echo Yii::app()->format->formatSize(115969);`
// displays: 113.25 KB

## Console application return code

You can now return integer in console application action and it will be used as application return code.

To learn more [refer to the definitive guide](http://www.yiiframework.com/doc/guide/1.1/en/topics.console#exit-codes).

## CJavaScript::encode() and js:

If you are using CJavaScript::encode() in your application with parameter coming from user input, your application is probably vulnerable. To avoid it set second argument to true:

`CJavaScript::encode($userInput, true);`

It will disable prefixing parameters with js:. If you need to pass JavaScript expression it's now preferrable to wrap these with CJavaScriptExpression:

`CJavaScript::encode(new CJavaScriptExpression('alert("Yii!");'), true);`

Note that second safe parameter doesn't affect CJavaScriptExpression in any way.

## HTTP caching

In addition to simply cache the output of an action, the new version of Yii introduces [CHttpCacheFilter](http://www.yiiframework.com/doc/api/1.1/CHttpCacheFilter/). This filter aids in setting HTTP headers to notify a client that a page's content has not been changed since the last request, so the server will not have to re-transmit the content. [CHttpCacheFilter](http://www.yiiframework.com/doc/api/CHttpCacheFilter) can be set up similar to [COutputCache](http://www.yiiframework.com/doc/api/COutputCache):

```
public function filters()
{
    return array(
        array(
            'CHttpCacheFilter + index',
            'lastModified'=>Yii::app()->db->createCommand("SELECT MAX(`update_time`) FROM {{post}}")->queryScalar(),
        ),
    );
}
```
[More details are available in the definitive guide](http://www.yiiframework.com/doc/guide/1.1/en/caching.page#http-caching)

## Model validation rules blacklisting

If you don't want to perform validation for some rule when particular scenarios are active you could specify except parameter containing their names. Syntax is the same as for on parameter:

// string with scenario names separated with commas (spaces are ignored)
`array('username', 'required', 'except'=>'ignore, this, scenarios, at-all',)`

[More details are available in the definitive guide](http://www.yiiframework.com/doc/guide/1.1/en/form.model#declaring-validation-rules)

## New tools and workflow for translation team

[New translation guidelines](https://github.com/yiisoft/yii/wiki/Documentation-translation-guidelines) are defining workflow for github-hosted translations. New tool can show you all changes since translation was updated last time. We hope it will help translation team.