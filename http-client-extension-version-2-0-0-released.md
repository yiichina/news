**HTTP 客户端扩展 2.0.0 版本发布了**

我们很高兴的宣布 HTTP 客户端扩展的初始版本 2.0.0 发布了。

此扩展允许执行 compose 和 HTTP 请求：

```
use yii\httpclient\Client;

$client = new Client();
$response = $client->createRequest()
    ->setMethod('post')
    ->setUrl('http://example.com/api/1.0/users')
    ->setData(['name' => 'John Doe', 'email' => 'johndoe@domain.com'])
    ->send();
if ($response->isOk) {
    $newUserId = $response->data['id'];
}
```
这是我们创建的最有争议的扩展，但是我们相信此扩展是值得发布的。

此扩展可以使用 Composer 进行安装：

`composer install yiisoft/yii2-httpclient:~2.0.0`

~2.0.0 版本约束条件必须使用，这是由于 2.1.0 版可能会终止向后兼容性。通过 [Composer 文档版本约束](https://getcomposer.org/doc/articles/versions.md#next-significant-release-operators) 来学习如何工作。

使用方法请参阅 [官方扩展说明](https://github.com/yiisoft/yii2-httpclient/blob/master/docs/guide/README.md)。

请注意 yii2-httpclient 不兼容 PSR-7 规范。考虑在 2.1.0 版中解决这个问题。