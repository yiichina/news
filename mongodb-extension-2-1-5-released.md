我们很高兴的宣布 MongoDB 扩展 2.1.6版本发布了，修复了两个bug:

* Fixed yii\mongodb\Command::aggregate() 在 MongoDB 3.6 服务器产生一个没有 'cursor' 选项的错误
* Fixed yii\mongodb\Collection::dropIndex() 在通过索引插件指定排序，将无法删除索引

参阅 [CHANGELOG](https://github.com/yiisoft/yii2-mongodb/blob/2.1.6/CHANGELOG.md) 查看更多细节。
