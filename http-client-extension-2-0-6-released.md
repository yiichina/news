我们很高兴的宣布 HTTP 客户端扩展 2.0.6 版本发布了，其中包含一个小的新功能和一个 bug 的修复：

* Message::getHeaders() 无法解析 HTTP 状态码，由于可能包含 : character
* Request::createFullUrl() 当结合使用 Client::$baseUrl 和 Request::$url 防止多条斜线的出现
参阅 [CHANGELOG](https://github.com/yiisoft/yii2-httpclient/blob/2.0.6/CHANGELOG.md) 查看更多细节。
