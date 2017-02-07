**HTTP 客户端扩展 2.0.2 发布了**

我们很高兴的宣布 HTTP 客户端扩展 2.0.2 版本发布了，此版本修复了以下重大错误：

* 解决了在使用特定 PHP 版本时 StreamTransport 收集 HTTP 响应头失败的问题。
* 解决 Response::detectFormatByContent(), 如果源内容中包含 | 符号，无法检测到 URL 编码格式的问题。

参阅 [CHANGELOG](https://github.com/yiisoft/yii2-httpclient/blob/2.0.2/CHANGELOG.md) 查看更多细节。