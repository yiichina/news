**Yii 2.0.10 发布了**

我们很高兴的宣布 Yii 框架 2.0.10 版本发布了。请参考说明 [http://www.yiiframework.com/download/](http://www.yiiframework.com/download/) 安装或升级到此版本。

2.0.10 版本是 Yii 2.0 的一次小的发布，其中包含大约 [80 个小的新功能和 bug 的修复](https://github.com/yiisoft/yii2/blob/2.0.10/framework/CHANGELOG.md)。

此版本中有两个小的更改可能会影响您的现有应用程序，所以请检查 [UPGRADE.md](https://github.com/yiisoft/yii2/blob/2.0.10/framework/UPGRADE.md) 文件。

感谢伟大的 [社区](https://github.com/yiisoft/yii2/graphs/contributors)，为我们提供有价值的 pull requests 和讨论。没有你的帮助就没有此次发布！

你可以通过 starring 或者 watching [Yii 2.0 GitHub](https://github.com/yiisoft/yii2) 项目来跟进 Yii 2 的开发进度。你也可以通过 Yii Twitter feeds 或者加入 Yii Facebook group 来联系其他的 Yii 开发成员。[论坛](http://www.yiiframework.com/forum/index.php/topic/72831-yii-2010-is-released/)中也有关于本次发布的新闻帖子。

下面我们总结了一些重要的关于此版本的 `features/fixes`。关于这个版本更新的完整列表，请参阅 [CHANGELOG](https://github.com/yiisoft/yii2/blob/2.0.10/framework/CHANGELOG.md)。

## 网址

`yii\web\UrlNormalizer` 允许规范化带有和不带有斜杠的请求，这对 SEO 非常重要。有关详情，请参阅权威指南中新的 [网址标准化](http://www.yiiframework.com/doc-2.0/guide-runtime-routing.html#url-normalization) 部分。

## 迁移

迁移有一些修复以及显著的改进。使用这个新版本，您可以通过启用命名空间迁移从多个位置运行迁移。为了启用此功能，你应该在控制台应用程序配置为控制器配置 `$migrationNamespaces` 属性：

```
return [
    'controllerMap' => [
        'migrate' => [
            'class' => 'yii\console\controllers\MigrateController',
            'migrationNamespaces' => [
                'app\migrations',
                'some\extension\migrations',
            ],
            //'migrationPath' => null, // 允许完全禁止非命名空间迁移
        ],
    ],
];
```

## 错误处理

会话错误不再在调试模式下静默，允许在开发过程中捕获所有问题。

## 请求

现在有一个便捷的方法 `yii\web\Request::getHostName()` ，它返回当前请求的主机名，所以你不必再手动去做了。

非 post 请求使用 `multipart/form-data` 如文件上传，现在可以使用 `yii\web\MultipartFormDataParser` 解析。为了使用它，你应该配置 `Request::parsers` 使用以下方式：

```
return [
    'components' => [
        'request' => [
            'parsers' => [
                'multipart/form-data' => 'yii\web\MultipartFormDataParser'
            ],
        ],
        // ...
    ],
    // ...
];
```
然后在访问 `$_FILES` 用前调用 `Request::getBodyParams()` 。

## 数据库

添加了一个新的 ActiveRecord 行为。`yii\behaviors\AttributeTypecastBehavior` 提供了自动模型属性类型转换的能力，允许保持严格的属性类型。

你应该通过 attributeTypes 指定确切的属性类型：

```
use yii\behaviors\AttributeTypecastBehavior;
 
class Item extends \yii\db\ActiveRecord
{
    public function behaviors()
    {
        return [
            'typecast' => [
                'class' => AttributeTypecastBehavior::className(),
                'attributeTypes' => [
                    'amount' => AttributeTypecastBehavior::TYPE_INTEGER,
                    'price' => AttributeTypecastBehavior::TYPE_FLOAT,
                    'is_active' => AttributeTypecastBehavior::TYPE_BOOLEAN,
                ],
                'typecastAfterValidate' => true,
                'typecastBeforeSave' => false,
                'typecastAfterFind' => false,
            ],
        ];
    }
 
    // ...
}
```
如果 attributeTypes 留空，它的值将根据所有者的验证规则自动被检测：

```
use yii\behaviors\AttributeTypecastBehavior;
 
class Item extends \yii\db\ActiveRecord
{
    public function rules()
    {
        return [
            ['amount', 'integer'],
            ['price', 'number'],
            ['is_active', 'boolean'],
        ];
    }
 
    public function behaviors()
    {
        return [
            'typecast' => [
                'class' => AttributeTypecastBehavior::className(),
                // 'attributeTypes' 将根据 `rules()` 自动生成
            ],
        ];
    }
 
    // ...
}
```
如果使用 Oracle, 现在有一 个 `yii\mutex\OracleMutex` 通过 Oracle 锁实现互斥“锁”机制。

## 控制台

控制台命令现在可以通过 `-h` 或 `--help` 被调用显示帮助信息。

## 测试

由于 Codeception 更改，调整了应用程序模板来反映它们。在Codeception网站阅读全新的[Yii 2.0 快速入门指南](http://codeception.com/for/yii) 。如果您使用的高级应用程序，请[参阅其测试文档](https://github.com/yiisoft/yii2-app-advanced/blob/master/docs/guide/start-testing.md)。
