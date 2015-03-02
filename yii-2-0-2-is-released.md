# Yii 2.0.2 发布了 #

我们很高兴地宣布 Yii Framework 2.0.2 版本发布了。请参考说明 http://www.yiiframework.com/download/ 安装或升级到此版本。

版本 2.0.2 是 Yii 2.0 的一个补丁版本。其中包含约40个次要的新功能和bug修复。更新的完整列表可以在 [更新日志]中找到。在此我们要感谢花宝贵的时间帮助提高 Yii 效率和使它能够发布的所有贡献者。

您可以遵循 Yii 2 的开发过程主要看 Yii 2.0 GitHub 的项目。您可以加入 Yii Twitter 组或者加入 Yii Facebook 组来联系其它 Yii 开发者。

下面我们总结了一些最重要的特征包含在本篇发布中。

## 路径别名

先前， 核心框架的代码只支持表示文件路径和网址别名。现在增加了对路径别名的支持。特别是， 您可以为路径创建别名，然后用别名来引用路径时，你需要为它创建一个URL。路径别名主要通过助手类方法 Url::to() 和 Url::toRoute() 来支持。例如，

```php
use yii\helpers\Url;
 
Yii::setAlias('@posts', 'post/index');
 
// /index.php?r=post/index
echo Url::to(['@posts']);
echo Url::toRoute('@posts');
```

您可能会发现路径别名是有用的当你的路径设计是不固定的你想避免处处更改URL创建代码而使你的路线设计被改变。

## 依赖组件配置

Many components contain properties that should be configured to be the ID of a dependent application component, 例如 yii\caching\DbCache::db, yii\web\CacheSession::cache。有时，to avoid introducing new application component or for unit testing purpose, you may want to directly configure such a property with a configuration array that can be used to create the dependent component. You can do so now like the following:

```php
$cache = Yii::createObject([
    'class' => 'yii\caching\DbCache',
    'db' => [
        'class' => 'yii\db\Connection',
        'dsn' => '...',
    ],
]);
```

如果你正在开发一个依赖于外部组件的新类， 您可以使用以下方法来容易得到类似的支持：

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

上面的代码将允许 db 属性被设置为以下任何初始值:

    一个字符串， 表示应用程序组件的 ID；
    一个 yii\db\Connection 实例；
    一个组件数组可用于创建 yii\db\Connection 实例。


## Immutable Slug

如果你使用 yii\behaviors\SluggableBehavior，现在你可以使用一个固定的名字命名一个新属性。通过设置该属性为 true，如果 slug 已经被创建，它将不再被改变，即使相应的源属性值改变。 这对 SEO 有特殊的目的用处因为你不想改变使用slug的公开的 URL。

## DatePicker Language Fallback

yii\jui\DatePicker widget 现在支持语言 fallback。This is useful when you configure its language property as a locale ID that contains region and/or variant part. 例如，if you set language to be de-DE, and the widget cannot find a language file named /ui/i18n/datepicker-de-DE.js, it will fall back to the language de and attempt the language file /ui/i18n/datepicker-de.js.

## 通过验证错误

yii\base\Model 类现在有一个简便的方法 addErrors() 它允许你从一个模型转换验证错误到另一个模型。例如， 如果有一个包含 ActiveRecord 模型类的一个表单模型类，你想通过 ActiveRecord 表单模型类验证错误，你可以只通过调用此方法：

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