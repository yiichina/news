** HTTP 客户端扩展 2.0.1 版发布了**

我们很高兴的宣布 HTTP 客户端扩展 2.0.1 版发布了，其中包含 10 个功能增强和 bug 的修复。详情请参阅 [CHANGELOG](https://github.com/yiisoft/yii2-httpclient/blob/2.0.1/CHANGELOG.md)。此次最大的改进是在 HTTP 请求时加入了 `beforeSend` 和 `afterSend` 监听事件：

```php
use yii\httpclient\Client;
use yii\httpclient\Request;
use yii\httpclient\RequestEvent;
 
$client = new Client();
 
$request = $client->createRequest()
    ->setMethod('get')
    ->setUrl('http://api.domain.com')
    ->setData(['param' => 'value']);
 
// Ensure signature generation based on final data set:
$request->on(Request::EVENT_BEFORE_SEND, function (RequestEvent $event) {
    $data = $event->request->getData();
 
    $signature = md5(http_build_query($data));
    $data['signature'] = $signature;
 
    $event->request->setData($data);
});
 
// Normalize response data:
$request->on(Request::EVENT_AFTER_SEND, function (RequestEvent $event) {
    $data = $event->response->getData();
 
    $data['content'] = base64_decode($data['encoded_content']);
 
    $event->response->setData($data);
});
 
$response = $request->send();
```
请[参考文档](https://github.com/yiisoft/yii2-httpclient/blob/master/docs/guide/README.md)了解更多。