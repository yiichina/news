# Yii 2.0.2 ������ #

���Ǻܸ��˵����� Yii Framework 2.0.2 �汾�����ˡ���ο� http://www.yiiframework.com/download/ ���˵����װ���������ð汾��

2.0.2 �汾�� Yii 2.0 ������һ���������а���Լ40����Ҫ���¹��ܺ�bug�޸����仯�������б�����ڸ�����־���ҵ����ڴ�����Ҫ��л�������ʱ�������� Yii Ч�ʺ�ʹ���ܹ����������й����ߡ�

You may follow the development progress of Yii 2 by starring or watching Yii 2.0 GitHub Project. You may also follow Yii Twitter feeds or join Yii Facebook group to connect with other Yii developers.

���������ܽ���һЩ����Ҫ�����������ڱ�ƪ�����С�

## ·������

Previously, the core framework code only supports aliases that represent file paths and URLs. It now adds the support for route aliases. In particular, you can create an alias for a route, and then use the alias to reference the route when you need to create a URL for it. ·��������Ҫͨ�������෽�� Url::to() �� Url::toRoute() ��֧�֡����磬

```php
use yii\helpers\Url;
 
Yii::setAlias('@posts', 'post/index');
 
// /index.php?r=post/index
echo Url::to(['@posts']);
echo Url::toRoute('@posts');
```

You may find route alias to be useful when your route design is not fixed and you want to avoid changing your URL creation code everywhere when your route design is changed.

## �����������

Many components contain properties that should be configured to be the ID of a dependent application component, such as yii\caching\DbCache::db, yii\web\CacheSession::cache. Sometimes, to avoid introducing new application component or for unit testing purpose, you may want to directly configure such a property with a configuration array that can be used to create the dependent component. You can do so now like the following:

```php
$cache = Yii::createObject([
    'class' => 'yii\caching\DbCache',
    'db' => [
        'class' => 'yii\db\Connection',
        'dsn' => '...',
    ],
]);
```

If you are developing a new class that depends on an external component, you may use the following approach to obtain the similar support readily:

```php
use yii\base\Object;
use yii\db\Connection;
use yii\di\Instance;
 
class MyClass extends Object 
{
    public $db = 'db';
 
    public function init() 
    {
        $this->db = Instance::ensure($this->db, Connection::className());
    }
}
```

The above code will allow the db property to be configured as any of the following initial values:

    a string, representing the ID of an application component;
    a yii\db\Connection instance;
    a configuration array that can be used to create a yii\db\Connection instance.


## Immutable Slug

If you are using yii\behaviors\SluggableBehavior, you can now make use of a new property named immutable. By setting this property to be true, if a slug has been generated before, it will no longer be changed, even if the corresponding source attribute value is changed. This is particular useful for SEO purpose because you do not want to change a published URL that uses the slug.

## DatePicker Language Fallback

The yii\jui\DatePicker widget now supports language fallback. This is useful when you configure its language property as a locale ID that contains region and/or variant part. For example, if you set language to be de-DE, and the widget cannot find a language file named /ui/i18n/datepicker-de-DE.js, it will fall back to the language de and attempt the language file /ui/i18n/datepicker-de.js.

## ͨ����֤����

The yii\base\Model class now has a convenient method addErrors() that allows you to transfer validation errors from one model to another. For example, if you have a form model class which contains an ActiveRecord model class, and you would like to pass the form validation errors to the ActiveRecord class, you can simply call this method:

```php
use yii\base\Model;
use yii\db\ActiveRecord;
 
class MyForm extends Model 
{
    public $model;
 
    public function process()
    {
        // ...
        if (!$this->validate()) {
            $this->model->addErrors($this->getErrors());
            // ....
        }
    }
}
```