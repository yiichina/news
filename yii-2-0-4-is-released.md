# Yii 2.0.4 发布了 #

我们很高兴地宣布 Yii Framework 2.0.4 版本发布了。请参考说明 http://www.yiiframework.com/download/ 安装或升级到此版本。

版本 2.0.4 是 Yii 2.0 的一个补丁版本。其中包含了大约 100 个小的新功能和 bug 修复。它修复了关于 CVE-2015-3397 的安全问题，使 IE6/IE7 用户免于可能的 XSS 攻击。出于这个原因，如果你的应用程序需要支持IE6 / IE7，你应该升级到这个版本。这个版本新增了一个叫做 EachValidator 的验证器，通过这个验证器你可以检验输入数组当中的每一个元素。yii\base\Model 类新增了 attributeHints()  方法，从而用户可以配置输入框提示（除了 input 标签）。当你使用 yii\db\Query 执行数据库查询，你可以使用 indexBy() 和 column()。它返回一个 被指定列 或者 其他计算出来的值（因为indexby支持回调函数做参数）所索引的一个 值类型数组。在这个版本中还有很多有用的添加和更改。请参考 [核心框架更新日志](https://github.com/yiisoft/yii2/blob/2.0.4/framework/CHANGELOG.md) 和 扩展更新日志。

我们在此感谢花宝贵的时间帮助提高 Yii 效率和使它能够发布的 [所有的贡献者](https://github.com/yiisoft/yii2/graphs/contributors)。

您可以通过收藏和关注 [Yii 2.0 GitHub 项目](https://github.com/yiisoft/yii2) 来了解 Yii 2 项目的最新进展。您可以加入 [Yii Twitter feeds](https://twitter.com/yiiframework) 或者加入 [Yii Facebook group](https://www.facebook.com/groups/yiitalk/) 来联系其它 Yii 开发者。
