我们非常高兴的宣布 [Queue 扩展](https://github.com/yiisoft/yii2-queue) 2.2.1 版本发布了。

此版本中有几个有趣的变化：

- 默认情况下，如果 PHP 版本支持，将使用最新的 amqp-lib。
- 现在有了AWS SQS FIFO支持。

在前一版本中还存在一个概念错误，即将句柄方法添加到 cli 队列类。它被移动到 `\yii\queue\sqs\Queue`。

[GitHub 提供](https://github.com/yiisoft/yii2-queue/blob/2.2.1/CHANGELOG.md) 完整的更新日志。