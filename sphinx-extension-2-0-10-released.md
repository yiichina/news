我们很高兴的宣布 Sphinx 扩展 2.0.10 版本发布了，其中包含 4 个小的新功能和 bug 的修复：

* 修复了 yii\sphinx\Schema::findColumns() 无法合并具有相同名称的字段和属性列
* 修复了 yii\sphinx\QueryBuilder::buildInCondition() 与 PHP 7.2 不兼容
* yii\sphinx\QueryBuilder::callSnippets() 现在自动将片段源转换为字符串
* yii\sphinx\QueryBuilder 现在支持在特定条件下使用可遍历对象

参阅 [CHANGELOG](https://github.com/yiisoft/yii2-sphinx/blob/2.0.10/CHANGELOG.md) 查看更多细节。
