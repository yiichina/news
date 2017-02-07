**认证客户端扩展 2.1.1 版本发布了**

我们很高兴的宣布认证客户端扩展 2.1.1 版本发布了。此版本修复了一个关键的错误：`\yii\authclient\BaseClient::createRequest()` 方法忽略了 `defaultRequestOptions` 和 `requestOptions`，在特别的情况下，这个是导致 Github 授权出错的原因。

此版本也提供了一些功能增强和 bug 的修复。关于这个版本更新的完整列表，请参阅 [CHANGELOG](https://github.com/yiisoft/yii2-authclient/blob/2.1.1/CHANGELOG.md)。