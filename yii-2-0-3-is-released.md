Yii 2.0.3 发布了

我们很高兴地宣布 Yii Framework version 2.0.3 版本发布了。请参考说明 http://www.yiiframework.com/download/ 安装或升级到此版本。

版本 2.0.3 是 Yii 2.0 的一个补丁版本。其中包含了大约 50 个小的新功能和 bug 修复。更新的完整列表可以在 [更新日志]中找到。在此我们要感谢花宝贵的时间帮助提高 Yii 效率和使它能够发布的所有贡献者。

还要感谢提高文档质量和将文档翻译成多国语言的贡献者。

您可以遵循 Yii 2 的开发过程主要看 Yii 2.0 GitHub 的项目。您可以加入 Yii Twitter 组或者加入 Yii Facebook 组来联系其它 Yii 开发者。

下面我们总结了一些最重要的特征包含在本篇发布中。

## 加密库变化

这是内部主要的一个比较大的变化。我们已经在 yii\base\Security 中用 OpenSSL 取代了 Mcrypt（它的作者已经8年没有更新它了）。感谢 Tom Worster 在保持高度安全和标准的 Yii 中所做的出色的工作。由于 OpenSSL 默认情况下内建于 PHP 中，所以你应该不会遇到任何向后兼容的问题。但是如果你这样做，请告诉我们。

## RBAC 缓存

如果您正在使用数据库来存储 RBAC 数据，你会发现它的执行并不理想，因为每个访问检查将涉及许多 SQL 语句的执行。为了提高性能，缓存机制正在实施对于 yii\rbac\DbManager。它存储在缓存中的整个 RBAC 层次结构中从而大大提高了 checkAccess() 的性能。默认情况下，RBAC 缓存未启用。您可以通过在应用程序配置中启用 yii\rbac\DbManager，如下：

```php
return [
    'components' => [
        'authManager' => [
            'class' => 'yii\rbac\DbManager',
            'cache' => 'cache',   // this enables RBAC caching
        ],
        'cache' => [
            'class' => 'yii\caching\ApcCache',
        ]
        // ...
    ],
]
```

## 页面缓存

先前，页面缓存仅被限制在 HTML 内容缓存中。如果你试图用它来响应缓存 RESTful ，你将会发现它将由于响应格式不正确而不工作。随着在此版本中的提高，现在你可以使用 yii\filters\PageCache 来缓存不同种类的响应数据以及响应头部。例如，在一个 RESTful 风格的控制器类中，你可以按如下方式缓存索引操作，

```php
public function behaviors()
{
    return [
        [
            'class' => 'yii\filters\PageCache',
            'only' => ['index'],
            'duration' => 60,
        ],
    ];
}
```

## Cache Busting for Assets

另一个增强型关联缓存支持 cache busting of published assets。经常发生在生产服务器中当你启用 JS 或者 CSS 文件的 HTTP 缓存时。缺点是如果您更改这些文件，由于 HTTP 缓存客户端仍然使用老版本。现在您可以通过配置 yii\web\AssetManager::appendTimestamp 来清除公共的 JS 和 CSS 文件缓存。通过设置该属性为 true，相应的 JS 和 CSS 文件的文件修改时间将被添加到每个 JS/CSS 网址中。因此，每当文件被修改，客户端将始终接收到最新版本：

```
return [
    'components' => [
        'assetManager' => [
            'class' => 'yii\web\AssetManager',
            'appendTimestamp' => true,
        ],
        // ...
    ],
]
```

## 修改当前的 URL

一个新的助手类方法 yii\helpers\Url::current() 的加入帮助您通过添加或删除一些 GET 参数更轻松地修改当前请求的 URL。例如，

```php
// assume $_GET = ['id' => 123, 'src' => 'google'], current route is "post/view"
 
// /index.php?r=post/view&id=123&src=google
echo Url::current();
 
// /index.php?r=post/view&id=123
echo Url::current(['src' => null]);
// /index.php?r=post/view&id=100&src=google
echo Url::current(['id' => 100]);
```

## 禁用日志轮转

如果您使用 yii\log\FileTarget 保留日志文件，现在您可以通过类似下面的单一属性配置来禁用日志自动轮转。当您使用外部工具轮转日志文件时这是最有用的。

```php
return [
    'components' => [
        'log' => [
            'targets' => [
                [
                    'class' => 'yii\log\FileTarget',
                    'enableRotation' => false,
                ],
            ],
        ],
    ],
];
```

## 数据属性

当您使用 yii\helpers\Html 来生成 HTML 标签，数据属性将被特殊处理。例如，

```php
// displays: <div data-name="xyz" data-age="20"></div>
echo Html::tag('div', '', ['data' => ['name' => 'xyz', 'age' => 20]]);
```

现在 ng 属性和 data-ng 属性通过此方法也被特殊处理。当您正在使用 AngularJS 时这是最有用的。对于所有其他的属性，数组将会转为 JSON 格式当被嵌入到 HTML 标签中时。

你可以通过修改 yii\helpers\Html::$dataAttributes 变量来配置哪一个属性应该像上面显示的一样被特别处理。

## Trimming Input

如果您使用的是 trim 验证规则，你将发现 trimming 也可用于客户端完成。也就是说如果一个 text input 和一个 trim 验证规则相关联，输入框数据两边的空格在input控件失去焦点的时候会被自动的过滤掉。如果你不喜欢这一新行为，您可以将 trim 验证规则的 enableClientValidation 选项设置为 false。

## 输入的最大长度

当使用 yii\helpers\Html::activeTextInput() 或者 yii\widgets\ActiveField::textInput() 来创建一个文件输入，你可能要指定生成的文本输入的 maxlength 属性。如果你将 string 验证规则与对应的 model 属性相关联，你可能希望避免显式指定最大长度的值，因为这可能已经在字符串规则中指定。您可以通过设置最大长度实现这一目标，它会自动填充字符串规则的最大选项的值。例如，

```php
// assume "name" has a validation rule: ['name', 'string', 'max' => 128]
// generates: <input type="text" ... maxlength="128">
echo Html::activeTextInput($model, 'name', ['maxlength' => true]);
```

## 配置对象

一个新接口名为 yii\base\Configurable 现在可以为配置生成一个类。如果一个类实现了这个接口，yii\di\Container 将会使构造函数的最后一个参数接收一个配置数组。例如，

```php
class Foo implements \yii\base\Configurable
{
    public function __construct($a, $b, $config = [])
    {
    }
}
 
$container = new \yii\di\Container;
$object = $container->get('Foo', [1, 2], ['prop1' => 3]);
// equivalent to: $object = new Foo(1, 2, ['prop1' => 3]);
```

在此版本之前，为了实现这一功能你将不得不从 yii\base\Object 中扩展延伸。