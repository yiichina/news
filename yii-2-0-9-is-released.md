**Yii 2.0.9 发布了**

我们很高兴的宣布 Yii 框架 2.0.9 版本发布了。请参考说明 http://www.yiiframework.com/download/ 安装或升级到此版本。

2.0.9 版本是 Yii 2.0 的一次小的发布，[其中包含大约 60 个小的新功能和 bug 的修复](https://github.com/yiisoft/yii2/blob/2.0.9/framework/CHANGELOG.md)。

此版本中有两个小的更改可能会影响您的现有应用程序，所以请检查 [UPGRADE.md](https://github.com/yiisoft/yii2/blob/2.0.9/framework/UPGRADE.md) 文件。

感谢伟大的 [Yii 社区](http://www.yiichina.com/)，为我们提供有价值的 pull requests 和讨论。没有你的帮助就没有此次发布！

你可以通过 starring 或者 watching [Yii 2.0 GitHub](https://github.com/yiisoft/yii2) 项目来跟进 Yii 2 的开发进度。你也可以通过 Yii Twitter feeds 或者加入 Yii Facebook group 来联系其他的 Yii 开发成员。[论坛](http://www.yiiframework.com/forum/index.php/topic/71580-yii-209-is-released/)中也有关于本次发布的新闻帖子。

下面我们总结了一些重要的关于此版本的 功能/修复。关于这个版本更新的完整列表，请参阅 [CHANGELOG](https://github.com/yiisoft/yii2/blob/2.0.9/framework/CHANGELOG.md)。

## Action filter

`\yii\base\ActionFilter` 现在仅仅支持除选项以外的通配符，这种特性在 `\yii\base\ActionFilter` 连接到其他模块或者应用的时候非常实用：
```
return [
    'as filter' => [
        'class' => 'app\filters\SomeFilter',
        'only' => [
            'particular/*', // all actions in controller 'particular'
            '*/captcha', // all 'captcha' actions in all controllers
        ],
    ],
    // ...
];
```

## Performance improvements

通过检测查询和添加适当的索引，提升了对数据库后端的数据转换性能。
Oracle 数据库架构的读取性能得到了提升。

## Schema builder and migrations

用于迁移的 Schema builder 得到了一些增强功能。

首先，Schema builder 有一个新的方法 null() 可以显式指定空值。当然，在默认值自动设置为 null 的时候，列将会自动设置为空值。
```
$type = $this->string(42)->null();
```
此外，Schema builder 添加了一个新方法用于在生成的 query 语句的后面添加自定义的 SQL。
```
$type = $this->string(15)->notNull()->append('collate ascii_bin')->append('character set ascii');
```
用于自动构建代码的迁移语法进行了一些调整，现在要求提供 _table 和 _column 的后缀。
```
./yii migrate/create create_user_table
./yii migrate/create add_name_column_to_user_table
```
## Data providers and widgets

这个版本在 grids 和 providers 上的增强都和标签有关。\yii\data\ArrayDataProvider 增加了一个 $modelClass 的属性，在数据数组为空的情况下可以使用该属性提供的标签。另外，用于定义所有数据列基础行为的\yii\grid\DataColumn，现在可以尝试从 grid 的 filterModel 中获取标签属性，除非无法从 filterModel 中获得他们。

## Refactorings

一个名为 CheckAccessInterface 的子接口从 RBAC ManagerInterface 中独立了出来，这个子接口可以被实现自定义访问的检测。

重构了 \yii\web\User::loginByCookie()，目的是为了更方便地去重写它。

## Assets

您可以将前端资源中的文件列表路径设置为 null 来告诉前端资源管理器不要去注册他们。这在开发环境中注册额外脚本的时候非常实用：
```
<?php
namespace common\assets;
use yii\web\AssetBundle;
 
class ReactAsset extends AssetBundle
{
    public $sourcePath = null;
 
    public $js = [
        YII_ENV_DEV ? "//fb.me/react-15.0.1.js" : "//fb.me/react-15.0.1.min.js",
        YII_ENV_DEV ? "//fb.me/react-dom-15.0.1.js" : "//fb.me/react-dom-15.0.1.min.js",
        YII_ENV_DEV ? "//cdnjs.cloudflare.com/ajax/libs/babel-core/5.6.15/browser.js" : null,
    ];
}
```
## Logging

`\yii\log\Target::$logVars` 现在将让被需要记录的日志提供更加细微的配置项：

- _SESSION - log 全局 session 变量。这和以前一样。
- _SESSION.id - 仅仅记录 session 的 id。
- !_SESSION.secret - 不记录 session 中的加密信息。
- 这些过滤规则被提取在 \yii\helpers\ArrayHelper::filter() 中，所以你能在你需要的时候使用它。

## Markdown

现在你可以通过调用 $defaultFlavor 为 yii\helpers\Markdown 配置默认风格。