我们很高兴地宣布 Yii 2.0.1 发布了。请参考说明 http://www.yiiframework.com/download/ 安装或升级到此版本。

版本 2.0.1 是 Yii 2.0 的一个补丁版本。其中包含了大约 90 个小的新功能和 bug 修复。更新的完整列表可以在 [更新日志](https://github.com/yiisoft/yii2/blob/2.0.1/framework/CHANGELOG.md) 中找到。除了代码的改进，也有很多文档的改进，尤其是 [Yii 2.0 的权威指南](http://www.yiiframework.com/doc-2.0/guide-index.html) 已被翻译成多种语言。在此，我们要感谢那些花宝贵时间帮助改进 [Yii 的贡献者](https://github.com/yiisoft/yii2/graphs/contributors)，因为你们才能使发布成为可能。

你可以关注 Yii 2 的开发进度，starring 或 watching [Yii 2.0 GitHub 项目](https://github.com/yiisoft/yii2)。你可以关注 [Yii Twitter 订阅](https://twitter.com/yiiframework)或者加入 [Yii Facebook 群](https://www.facebook.com/groups/yiitalk/) 去接触其他 Yii 开发者。

下面我们总结了一些包含在这个版本最重要的特点。

## 强制 Asset 转换

Asset 包支持自动 asset 转换, 例如转换 LESS 为  CSS。然而在源 assets 变更时适当的检测资源的代价是昂贵的，特别是一个资源被另一个导入。为了解决这个问题，您现在只需要配置 assetManager 如下总是强制转换 assets：

```php
[
    'components' =>  [
        'assetManager' => [
            'converter' => [
                'forceConversion' => true,
            ]
        ]
    ]
];
```

## 选择子查询

查询生成器支持在不同的地方使用子查询。现在，您还可以在SELECT部分使用子查询。例如，

```php
$subQuery = (new Query)->select('COUNT(*)')->from('user');
$query = (new Query)->select(['id', 'count' => $subQuery])->from('post');
// $query represents the following SQL:
// SELECT `id`, (SELECT COUNT(*) FROM `user`) AS `count` FROM `post`
```

## 防止在 AJAX 中加载 CSS

此前，Yii 有防止在 AJAX 响应加载同一个 JavaScript 文件的支持。它现在还支持防止在 AJAX 响应加载相同的 CSS 文件。要使用此功能，您只需要像下面这样简单地使用 YiiAsset 资源包：

```php
yii\web\YiiAsset::register($view);
```

## 刷新架构缓存

一个新的控制台命令添加了，允许您刷新架构缓存。这在更改生产服务器的代码部署导致DB模式是非常有用的。只需运行命令，如下所示：

```
yii cache/flush-schema
```

## 增强助手类

Html::cssFile() 方法现在支持 noscript 选项，这将把生成的链接标签在一个noscript标记。您也可以在配置 asset 包的 AssetBundle::cssOptions 属性时使用此选项。例如，

```php
use yii\helpers\Html;
 
echo Html::cssFile('/css/jquery.fileupload-noscript.css', ['noscript' => true]);
```

之前的 StringHelper::truncate() 只支持截断纯文本的字符串截取。它现在支持截取 HTML 字符串，并确保截取的结果仍然是一个有效的 HTML 字符串。
Inflector 类得到一个新的方法名字为 sentence() 可以连接几个词成句。例如，

```php
use yii\helpers\Inflector;
 
$words = ['Spain', 'France'];
echo Inflector::sentence($words);
// output: Spain and France
 
$words = ['Spain', 'France', 'Italy'];
echo Inflector::sentence($words);
// output: Spain, France and Italy
 
$words = ['Spain', 'France', 'Italy'];
echo Inflector::sentence($words, ' & ');
// output: Spain, France & Italy
```

## 增强 Bootstrap 扩展

首先，Twitter Bootstrap 升级到了版本 3.3.x。如果你想坚持使用旧版本，你可以明确在你的项目中 composer.json 文件中指定。

新特性增加了几个 Bootstrap 小部件。请参阅 [类参考](http://www.yiiframework.com/doc-2.0/ext-bootstrap-index.html) 更多详细信息。

    yii\bootstrap\ButtonDropdown::$containerOptions
    yii\bootstrap\Modal::$headerOptions
    yii\bootstrap\Modal::$footerOptions
    yii\bootstrap\Tabs::renderTabContent
    yii\bootstrap\ButtonDropdown::$containerOptions

## 增强 MongoDB 扩展

findAndModify 操作现在由 yii\mongodb\Query 和 yii\mongodb\ActiveQuery 支持。例如，

```php
User::find()->where(['status' => 'new'])->modify(['status' => 'processing']);
```

调试面板同样添加显示MongoDB 查询执行。要使用此面板，只需要像下面这样配置 Yii 调试器，

```php
[
    'class' => 'yii\debug\Module',
    'panels' => [
        'mongodb' => [
            'class' => 'yii\mongodb\debug\MongoDbPanel',
        ]
    ],
]
```

## 增强 Redis 扩展

在 Yii 的 Redis 的扩展现在支持使用 UNIX socket 连接，它相比基于TCP的连接能快 50％。要使用它，配置 Redis 的连接如下：

```php
[
    'class' => 'yii\redis\Connection',
    'unixSocket' => '/var/run/redis/redis.sock',
]
```