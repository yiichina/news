**认证客户端扩展 2.1.0 版本发布了**

我们很高兴的宣布认证客户端扩展 2.1.0 版本发布了。这次主要的改进是使用 [HTTP 客户端扩展](https://github.com/yiisoft/yii2-httpclient)来为 [REST API 请求](https://github.com/yiisoft/yii2-authclient/blob/master/docs/guide/usage-api.md)提供更成熟的基础。

注意是 2.1.0 版引入了重要的向后兼容性符号！现有的基于 2.0.x 版写的代码不经过任何调整，很可能不能很好地运行在 2.1.0 版上。

检查从 2.0.x 版本迁移的升级说明。如果你想留在 2.0.x 版本请确认你的 composer.json 有必要的版本约束限制，例如 ~2.0.6。请查看 [Composer 文档](https://getcomposer.org/doc/articles/versions.md)来了解是怎样进行版本约束的。

此版也包含一些功能的增强，像 `client_credentials` 的支持和 OAuth 2.0 的 `password` 认证授权类型。关于这个版本更新的完整列表，请参阅 [CHANGELOG](https://github.com/yiisoft/yii2-authclient/blob/2.1.0/CHANGELOG.md)。