**新的官方扩展发布了**

我们很高兴的宣布扩展发布了。正如你可能已经知道的，我们决定将扩展的发布从框架的发布中分离出来，来允许更加灵活并且可能会更加频繁的发布扩展。然而过去并没有这样做，因为我们忙于框架本身并且每一个版本都需要一些额外的资源来检查代码并准备发布。我们现在已经自动化大部分扩展的发布过程，因此我们希望将来可以经常发布，现在很高兴的宣布以下扩展的发布：

* API 文档生成器 ([yii2-apidoc](https://github.com/yiisoft/yii2-apidoc/)) version 2.0.5, [CHANGELOG](https://github.com/yiisoft/yii2-apidoc/blob/2.0.5/CHANGELOG.md)
* Bootstrap 扩展 ([yii2-bootstrap](https://github.com/yiisoft/yii2-bootstrap/)) version 2.0.6, [CHANGELOG](https://github.com/yiisoft/yii2-bootstrap/blob/2.0.6/CHANGELOG.md)
* Codeception 扩展 ([yii2-codeception](https://github.com/yiisoft/yii2-codeception/)) version 2.0.5, [CHANGELOG](https://github.com/yiisoft/yii2-codeception/blob/2.0.5/CHANGELOG.md)
* Debug 工具栏 ([yii2-debug](https://github.com/yiisoft/yii2-debug/)) version 2.0.6, [CHANGELOG](https://github.com/yiisoft/yii2-debug/blob/2.0.6/CHANGELOG.md)
* Elasticsearch 扩展 ([yii2-elasticsearch](https://github.com/yiisoft/yii2-elasticsearch/)) version 2.0.4, [CHANGELOG](https://github.com/yiisoft/yii2-elasticsearch/blob/2.0.4/CHANGELOG.md)
* Gii 代码生成器 ([yii2-gii](https://github.com/yiisoft/yii2-gii/)) version 2.0.5, [CHANGELOG](https://github.com/yiisoft/yii2-gii/blob/2.0.5/CHANGELOG.md)
* jQuery UI 扩展 ([yii2-jui](https://github.com/yiisoft/yii2-jui/)) version 2.0.5, [CHANGELOG](https://github.com/yiisoft/yii2-jui/blob/2.0.5/CHANGELOG.md)
* Redis 扩展 ([yii2-redis](https://github.com/yiisoft/yii2-redis/)) version 2.0.5, [CHANGELOG](https://github.com/yiisoft/yii2-redis/blob/2.0.5/CHANGELOG.md)
* SwiftMailer 扩展 ([yii2-swiftmailer](https://github.com/yiisoft/yii2-swiftmailer/)) version 2.0.5, [CHANGELOG](https://github.com/yiisoft/yii2-swiftmailer/blob/2.0.5/CHANGELOG.md)
* Smarty 扩展 ([yii2-smarty](https://github.com/yiisoft/yii2-smarty/)) version 2.0.5, [CHANGELOG](https://github.com/yiisoft/yii2-smarty/blob/2.0.5/CHANGELOG.md)

以上发布包含了许多 bug 修复和新功能。下面我们介绍一些显著变化。

关于评论和反馈，[forum thread](http://www.yiiframework.com/forum/index.php/topic/70208-new-official-extension-releases/) 中有关于这条新闻的公告。

## API 文档生成器

API 文档生成器通过允许将类名做为模板参数而变得更易于扩：

`vendor/bin/apidoc guide source/docs ./output --template=app\apidoc\MyTemplateClass`

通过这种方法你可以使用你自己定义的渲染器This way you can use your own custom renderer which is found by autoloading the template renderer class.

json 模板也被添加到 json 格式提供的类结构中。

最后关于提示信息的样式，指南文档中已经改进了 Note 和 Warning 模块。

## Bootstrap

在 bug 修复中，此版本为 Bootstrap checkbox/radio 切换按钮添加了一个新的 bootstrap 小部件：ToggleButtonGroup。

你可以通过在 ActiveForm 中的 widget() 方法来使用此小部件，比如像这样：

```php
<?= $form->field($model, 'item_id')->widget(\yii\bootstrap\ToggleButtonGroup::classname(), [
     // configure additional widget properties here
]) ?>
```

## Debug 工具栏

debug 工具栏做了一个主要改进，即可以在同一浏览器窗口中工作。这使得，当在同一 tab 下，就像你所用的众所周知的浏览器调试工具一样，你可以更加容易的调试你的 Yii 应用程序。

在其它稳定的改进中，debug 工具栏现象也从 `assert manager` 中独立了出来, 使它可以在更多的自定义环境中工作，因此 `ToolbarAsset` 类已经被删除。

## Elasticsearch 扩展

elasticsearch 扩展兼容新的 Elasticsearch 2.0 版本和其它方面的改进，例如 HTTP 认证，当连接到 Elasticsearch，AWS Elasticsearch 服务兼容性，类似 min_score 的自定义查询选项。

我们还增加了对 Elasticsearch scroll API 的支持，提供了 batch() 和 each() 查询方法，这些方法也可以从 SQL Query 接口中作更深入的了解。同时，新的版本也修订了 `updateAll` 和 `deleteAll` 方法默认限制十条记录的问题。

## Gii 代码生成器

新选项已经添加到 generator 类中。同时，现在提供了一个新的选项，用于当生成 CURD 文件时将 `GridView` 用 `Pjax` 包装。Model 生成器现在会根据表的外键来生成规则。同样的，它也可以生成反向关系。

现在你可以不需要选中任何东西，使用 CTRL+C 组合键就可以在生成器界面获得全部的预览生成代码，通过这个方法，使用生成的代码变得越来越容易。