我们很高兴的宣布 Yii 框架 2.0.12 版本发布了。请参考说明 http://www.yiiframework.com/download/ 安装或升级到此版本。

2.0.12 版本是 Yii 2.0 的较小的发行版，它包含了超过 [100 处的增强和 bug 修复](https://github.com/yiisoft/yii2/blob/2.0.12/framework/CHANGELOG.md)。

有几个微小的变化可能影响到你的现有的应用程序，所以一定要检查 [UPGRADE.md](https://github.com/yiisoft/yii2/blob/2.0.12/framework/UPGRADE.md) 文件。

感谢 Yii [社区](https://github.com/yiisoft/yii2/graphs/contributors)对我们项目的支持，我们共同完成了它！

你可以通过 star 或者一直留意着 Yii 2.0 GitHub 项目来了解 Yii 2 的发展历程。你也可以[关注 Yii 的推特](https://twitter.com/yiiframework)或者加入 [Yii Facebook 群组](https://www.facebook.com/groups/yiitalk/)来和其他 Yii 开发者保持联系。还有一个关于本新闻公告的[论坛帖子](http://www.yiiframework.com/forum/index.php/topic/74896-yii-2012-is-released/)。

这个版本的发布比预期的时间要长一点，因为我们最近忙于其他一些事情。比如，[一个 Yii 会议将在莫斯科举行](https://yiiconf.ru/)，我们要在两个星期内新建一个[网站](https://github.com/yiisoft-contrib/yiiframework.com)。

Since there's [Yii 2.1 in development](https://github.com/yiisoft/yii2/tree/2.1), make sure you have `~2.0.12` in your `composer.json` instead of >= or * so when next major version of Yii is released, your project won't break by itself.

Below we summarize some of most important features/fixes included in this release. 关于变化的完整列表可以在 [CHANGELOG](https://github.com/yiisoft/yii2/blob/2.0.12/framework/CHANGELOG.md) 中找到。

# 测试

Test coverage is very important to detect stability issues early. Thanks to [@vladis84](https://github.com/vladis84), [@boboldehampsink](https://github.com/boboldehampsink), [@Kolyunya](https://github.com/Kolyunya) and other community members Yii got code covered by more unit tests.

Additionally, [@schmunk42](https://github.com/schmunk42) set up an additional docker-based [testing environment at GitLab](https://gitlab.com/yiisoft/yii2/pipelines). Some tests are faling there. Mainly because of differences in i18n data. This will be fixed later.

# 数据库

Database support got a few enhancements regarding using expressions. Now you can use them in `\yii\db\QueryTrait::limit()`, `\yii\db\QueryTrait::offset()`, and `\yii\data\Sort`.

# MSSQL

MSSQL support was enhanced. First, schema reading performance was increased signficantly. Second, `yii\db\mssql\QueryBuilder::resetSequence()` was implemented.

# 安全

* `yii\base\Security::hkdf()` was improved to take advantage of native hash_hkdf() implementation in PHP >= 7.1.2.
* `mt_rand()` is now used instead of rand() in `yii\captcha\CaptchaAction`.

# 可用性

Migrations template was changed to use `safeUp()` and `safeDown()`. In case changing schema in transactions is not supported, such as in case with MySQL, it silently executes migration methods wihtout transaction.

Various framework components got more sane defaults:

* `\yii\data\SqlDataProvider` now provides automatic fallback for the case when totalCount is not specified.
* Data provider now automatically sets an ID so there is no need to set it manually in case multiple data providers are used with pagination.
* `yii\grid\DataColumn` filter is automatically generated as dropdown list in case of format set to boolean.

yii cache command now warns about the fact that it's not able to flush APC cache from console.

`yii\filters\AccessRule` now allows passing parameters to the role checking function.

# 性能

* Added support for caching of `yii\web\UrlRule::createUrl()` results in `yii\web\UrlManager` for rules with defaults.
* Added option to disable query logging and profiling in DB command
* `yii\data\ActiveDataProvider` no longer queries models if models count is zero.

# 附加功能

`StringHelper got URL-safe base64 encode()/decode()` 方法。Should be useful for various tokens.

`yii\helpers\Html::img()` now allows you to specify srcset:

```php
echo Html::img('/base-url', [
    'srcset' => [
        '100w' => '/example-100w',
        '500w' => '/example-500w',
        '1500w' => '/example-1500w',
    ],
]);
```

It's now possible to render current yii\widgets\LinkPager page disabled by setting disableCurrentPageButton to true.

访问控制和验证器要求更少的依赖性：

* `yii\filters\AccessControl` 现在可以在没有用户组件的情况下使用。
* 核心验证器不再需要 `Yii::$app` 去设置。
