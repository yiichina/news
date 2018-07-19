我们很高兴的宣布 [redis 扩展](https://github.com/yiisoft/yii2-redis) 2.0.6 版本发布了。此扩展介绍了几种 bug 修复和功能增强。

由于在保存前和加载记录后计算的 key 不同，所以复合主键的 ActiveRecord 没有被正确保存。此版本已经修复了这个问题。如果使用复合主键的AR，请确保更新后的 implementation 工作正常。

修复了两个执行命令 (例如 CLIENT LIST 和 SCRIPT LOAD) ，这两个执行命令以前是失败的。Also PHPdoc @method annotations for redis methods have been added which make it easier to use the commands directly as PHP methods, e.g. `$redis->clientList()`. 这个以前是可以实现的，但是现在 IDEs 的自动完成工作更容易了。

现在还可以将  `Connection::$database`  设置为 null 来避免连接成功后发送一个 `SELECT` 命令。这对于在不支持选择多个数据库的一些代理环境中使用 redis 非常有用。

This release also adds support for `\yii\db\QueryInterface::emulateExecution()` which has been introduced in Yii 2.0.11 to allow creating an empty query without the need to connect to a database.

最后添加了一个 Mutex 类它实现了一个基于 mutex 的 Redis。

参阅 [CHANGELOG](https://github.com/yiisoft/yii2-redis/blob/2.0.6/CHANGELOG.md) 查看完整的更新列表。

感谢此次发布的所有贡献者!
