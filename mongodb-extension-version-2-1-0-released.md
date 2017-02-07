**MongoDB 扩展 2.1.0 版本发布了**

我们很高兴的宣布 MongoDB 扩展 2.1.0 版本发布了，此版本使用新的 [PHP MongoDB 驱动](https://secure.php.net/manual/en/set.mongodb.php)，提供了在 PHP 7.x 和 HHVM 上运行的能力。

注意这个新版 MongoDB 驱动的升级向后不兼容！很有可能，现有的基于 2.0.x 版本的代码如果不作调整将不能在 2.1.0 上运行。检查从 2.0.x 版本迁移的 [升级](https://github.com/yiisoft/yii2-mongodb/blob/2.1.0/UPGRADE.md) 说明。

如果你想继续使用 2.0.x 这个低版本的驱动，需要确保你的 composer.json 有必要的版本约束限制，例如 ~2.0.5。检查 Composer 文档中的 [版本约束](https://getcomposer.org/doc/articles/versions.md#next-significant-release-operators) 一节来学习它是如何工作的。

此版本还提供了几个增强功能，如批量写入和读取数据。关于这个版本更新的完整列表，请参阅 [CHANGELOG](https://github.com/yiisoft/yii2-mongodb/blob/2.1.0/CHANGELOG.md)。