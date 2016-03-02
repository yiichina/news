我们很高兴地宣布 Yii Framework 1.1.17 版本发布了。你可以从 [yiichina.com/download/](http://www.yiichina.com/download) 下载。

在这次发布中，其中包含36个小的新功能和 bug 修复。关于这个版本更新的完整列表，请参阅 [更新日志](https://raw.githubusercontent.com/yiisoft/yii/1.1.17/CHANGELOG)。注意这个版本包含了一个关于 APCu 缓存的重大更新，所以如果你使用它时，请阅读 [升级说明](https://raw.githubusercontent.com/yiisoft/yii/1.1.17/UPGRADE) 来学习如何升级。

感谢花宝贵的时间帮助提高 Yii 效率和使它能够发布的 [所有贡献者](https://github.com/yiisoft/yii/graphs/contributors)。

注意 1.1.17 版本是 [1.1 版本更新的最终版](http://www.yiichina.com/news/91)。我们目前正在积极开发和维护代表新一代利用最新技术的 PHP 框架 Yii 2.0。你可以通过在 GitHub 上 starring 或者 watching [Yii 2.0 GitHub 项目](https://github.com/yiisoft/yii2) 来跟踪 Yii 2 的开发进度。你可以通过 Yii Twitter feeds 或者加入 Yii Facebook group 来联系其他的 Yii 开发者。

这个版本的主要新功能包括：

* PHP7 兼容性。 
* CHttpRequest 现在可以自动把 content-type 为 application/json 的内容按照 JSON 解析。 
* 现在提供了基于数据库的 StatePersister 的实现。 
* Autoloader 现在处理不存在的添加了命名空间的类的时候不会产生错误提示，这样其他的 autoloaders 可以有机会处理这些类。 

你可以在 [相关论坛的帖子中](http://www.yiiframework.com/forum/index.php/topic/69279-yii-1117-is-released/) 评论这条新闻。