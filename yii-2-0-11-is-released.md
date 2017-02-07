**Yii 2.0.11 发布了**

我们很高兴的宣布 Yii 框架 2.0.11 版本发布了。请参考说明 http://www.yiiframework.com/download/ 安装或升级到此版本。

2.0.11 版本是 Yii 2.0 的较小的发行版，它包含了超过 110 处的增强和 bug 修复。

有四个微小的变化可能影响到你的现有的应用程序，所以一定要检查 UPGRADE.md 文件。

非常感谢我们优秀的社区， 我们共同完成了它。

你可能通过 star 或者一直留意着 Yii 2.0 GitHub 项目跟上了 Yii 2 的发展历程。你也可能关注了 Yii 的推特或者加入了 Yii Facebook 群组来和其他 Yii 开发者保持联系。也有一个关于本新闻公告的论坛帖子。

因为 Yii 2.1 已经在开发之中了，确保你的 `composer.json` 中写着 `~2.0.11` 而不是  `>=` 或者 `*` ，这样当下一个 Yii 主版本发行时，你的项目不会自己损坏。

下面总结一下包含在本次发行中最重要的功能/修复。关于变化的完整列表可以在 CHANGELOG 中找到。


## 测试覆盖

我们决定如果没有进行极少数特例的单元检测，那么我们就不接受 pull 请求。这样会带来更高的代码质量，以及更少的代码审查时间。 超过半数的 2.0.11 pull 请求合并是按照这个规矩来的。

一些测试明显是重构的，比如对 URL mannger 的测试。测试的方法变得更小并且更容易理解。

Alexey Rogachev 在修订框架的 JavaScript 部分和增加测试来修复过程中的 bug 上做得非常好。多亏了它，JavaScript 现在被测试完美地覆盖到了，因此我们可以期待更好的框架稳定性。


## 控制台

控制台已经得到了 Bash and Zsh 对于./yii 命令的支持。 在手册中有对如何安装它的介绍。

另一个对可用性增强的是，当一个命令因为语法错误没能找到时，控制台脚本现在可以给出可供选择项了。


## Cache

你现在可以通过 yii\caching\Cache::$defaultDuration 来在一个地方为缓存指明默认全局持续时间了，

现在也有一个便利的快捷方式来进行典型的数据缓存工作：

```
$data = $cache->getOrSet($key, function () {
    return $this->calculateSomething();
});
```

与下面写法等价：

```
$data = $cache->get($key);
if ($data === false) {
    $data = $this->calculateSomething();
    $cache->set($key, $data);
}
```

##  配置

在大量的讨论之后，我们决定引入通过应用配置来设置 dependencies container 这一功能：

```
$config = [
    'id' => 'basic',
    // ...
    'container' => [
        'definitions' => [
            'yii\widgets\LinkPager' => ['maxButtonCount' => 5]
        ],
        'singletons' => [
        ],
    ],
];
```

参考官方手册中的 "应用配置" 一节来获取该主题的更多信息。

## 可用性和快捷方式

在每个发行版中，我们都希望提升错误信息的可用性，因为这会使得整个开发体验更加舒服。本次发行也不例外，当你尝试调用一个不存在的组件时，一个错误会非常清楚地告诉你发生了什么。在这之前，只是给你一个模糊的提示，告诉你自动加载失败了。

Controller 类有了两个快捷方法 `asJson()` 和 `asXml()` 来分别返回 JSON 或者 XML 数据。之前这也是能做到的，但是这两个方法增加了语法糖，使得常见情况更加简单。


## 性能

在本发行版中， Yii 有了一些性能增强。

-当处理空关系时，数据库不再发出带有 0=1 条件的查询。
-当角色分配是空的时候，RBAC跳过递归检查。
- 唯一性验证器只挑选主键。
另一个增强并不能立即提高性能但是也有助于性能的提升， 我们已经开始记录内存使用和数据的路由，因此可以期待下个发行版中的 debug 模块中的新面板。

## 数据库

添加了三个新方法：`yii\db\Query: filterHaving()` 、`andFilterHaving()` 和 `orFilterHaving()`。它们和其他的 `filter*` 方法一样，都是只当传给它们的值不是空的情况下才增加条件。这些方法典型用法是在备份筛选表单的 model 中使用。

`yii\db\Connection` 有了几个增强，当你处理主从设置时会尤其有用。

新增的  `shuffleMasters`  属性，提供了禁止洗牌主库连接的功能。
新增的 `getMaster()` getter 方法和 `master` 属性可以用来获得当前活跃主库连接。
`\yii\db\Query` 可以被作为第二个参数或者参数值来传给 database command 的 `insert()` 方法。

```
$db = Yii::$app->db;
 
// insert 查询
 
$sourceQuery = new \yii\db\Query()
    ->select([
        'title',
        'content',
    ])->from('{{post_queue}}');
 
$command = $db->createCommand();
$command->insert('{{post}}', $sourceQuery);
 
// 将 query 当作普通值
 
$titleQuery = new \yii\db\Query()
    ->select('title')->from('{{titles}}')->limit(1);
 
$command = $db->createCommand();
$command->insert('{{post}}', [
    'title' => $titleQuery,
    'content' => 'Hello!',
]);
```

## PHP 7 兼容性

在每个发行版中，我们都确保一切都能在 PHP 最新的稳定版本 7.0 和 7.1 中运行，在 2.0.11 中，我们发现了一个关于错误处理器以及使用 Throwable 的问题，不过现在已经修复了。

## URL manager

当使用 `UrlManager::createAbsoluteUrl()`、`Url::to()` or `Url::toRoute()` 来生成 URLs 时，指定模式为空字符串现在会生成协议相对的 URLs：


```
echo Url::to('@web/images/logo.gif', '');
// 将打印 //www.example.com/images/logo.gif
```

现在 Url 规则现在能支持协议相对 URL 了，这使得 URL 能够更轻易地处理那些支持 HTTP 和 HTTPs 的应用了。

同时， 当创建 URLs 时，空的默认参数不再是必须的了：

```
echo Url::to(['post/index', 'page' => 1, 'tag' => '']);
 
// 现在可以是:
 
echo Url::to(['post/index', 'page' => 1]);
```


## 挂件

通过在 init 和 render 之前之后 激发事件，部件的可扩展性大大地被提高了。请参考发行说明来了解一些它能派上用场的例子。


##安全

现在有一个针对主机头攻击的 PHP 解决方案。 理想情况下，它应该能通过配置 web 服务器来被正确地解决，但是因为它是被请求的，并且它似乎是一个共享主机中十分常见的问题，我们给 HostControl filter 增加了一些功能。你可以在手册中阅读到配置相关。

存在一个问题，是关于在 debug 模式中逃逸的请求数据。因为这个问题影响开发模式，而不影响生产模式，所以我们决定不去开发一个独立的发行版来修复它。


## Composer 安装

此发行版中，我们也推出了一个 Yii 新版本 (2.0.5) 的 Composer 安装。对于那些不知道的人，该 composer 插件能够跟踪所有已安装的扩展和提供较少配置的引导机制。当一个新项目通过 composer 创建时，它也给正在运行的任务增加了一些钩子。多亏了 Robert Korulczyk，结合发行版，在 composer 安装好项目之后也能执行任务。这对于处理本地配置文件非常有用，现在新的复制文件的方法也可能使用了。查看 README 获取更多信息。

因为该发行版本中，该插件将比之前更可见，因为现在当你更新 `yiisoft/yii2`  包时，它会通知你 UPGRADE.md 中最新的提示。


## 带签名的提交和标签

这是第一个带有 GPG 签名标签的发行版，因此你可以证实代码是由 Yii 团队所发行的。今后我们将进一步探索该功能，也发布关于如何验证您的之后所得到代码的详细说明。

你已经能够以 github 中一个小的 "verified" 标签的形式来看到此变化了：

https://github.com/yiisoft/yii2-framework/releases/tag/2.0.11。