**Yii 2.0.7 发布了**

我们很高兴地宣布 Yii Framework 2.0.7 版本发布了。请参阅说明 [http://www.yiiframework.com/download/](http://www.yiiframework.com/download/) 安装或升级到此版本。

2.0.7 版本是 Yii 2.0 的一个补丁发布，包含100多个小的新功能和 bug 的修复以及许多改进的文档和指南翻译。

升级的额外步骤请参阅 UPGRADE.md 文件。

感谢我们伟大的 Yii 社区使我们得到宝贵的 pull requests 和讨论，使得这个版本能够发布。谢谢！

你可以通过在 GitHub 上 starring 或者 watching Yii 2.0 GitHub 项目来跟踪 Yii 2 的开发进度。你可以通过 Yii Twitter feeds 或者加入 Yii Facebook group 来联系其他的 Yii 开发者。

下面我们总结了一些在本次发布中的最重要的功能/修复。

## IP 验证器

现在有新的 IP 地址验证器，ranges 和 masks。它可以用作单独的验证器或者作为模型中 rules() 方法的一部分：

```
public function rules()
{
    return [
        ['address', 'ip', 'ranges' => [
             '192.168.10.128'
             '!192.168.10.0/24',
             'any' // allows any other IP addresses
        ]],
    ];
}
```
你可以通过查看指南的相关章节，类注释，以及测试用例，来了解它能做什么。

## i18n

Formatter 组件多了一个新的 `asDuration()` 方法，用来从表示为 DateInterval 对象、秒数或者 ISO8601 字符串的时间间隔获得一个可读性好的字符串：

```
echo Yii::$app->formatter->asDuration(131);
// outputs "2 minutes, 11 seconds"
Another enhancement is an ability to choose the calendar that would be used for date formatting. You can do so by setting the yii\i18n\Formatter::$calendar property. The following is an example of using Persian calendar:

Yii::$app->formatter->locale = 'fa_IR@calendar=persian';
Yii::$app->formatter->calendar = \IntlDateFormatter::TRADITIONAL;
Yii::$app->formatter->timeZone = 'UTC';
$value = 1451606400; // Fri, 01 Jan 2016 00:00:00 (UTC)
echo Yii::$app->formatter->asDate($value, 'php:Y');
// outputs "۱۳۹۴"
```
详情请查看[类参考手册](http://www.yiiframework.com/doc-2.0/yii-i18n-formatter.html#$calendar-detail)。

除此之外，非 URL 字符串的特定音译现在可以使用 [`Inflector::transliterate()`](http://www.yiiframework.com/doc-2.0/yii-helpers-baseinflector.html#transliterate()-detail) 来显示，可用于为越南等语言生成关键字和其他元数据。

## 数据库

除了修复之外还有一些显著的增强。你可以在 `Query::groupBy()` 和 `Query::orderBy()` 中使用 `yii\db\Expression`：

```
$expression = new Expression('SUBSTR(name, 2)');
$users = (new \yii\db\Query)
    ->from('user')
    ->orderBy($expression)
    ->limit(10)
    ->all();
```

关于 SQLite，现在你可以在 DSN 中使用别名：

```
'db' => [
    'dsn' => 'sqlite:@app/db/database.sqlite3',
]
```

在 active record 的 relations 关联操作中，对于表的别名的指定，我们增加了一个更简单的方法。现在可以对 `joinWith()` 使用和 `join()` 相似的语法：

```
// join the orders relation and sort the result by orders.id
$query->joinWith(['orders o'])->orderBy('o.id');
```

## 新的迁移语法的增强

在 2.0.6 版本中介绍的新的迁移语法得到了一些增强。首先，unsigned 的支持：

```
'createdBy' => $this->integer(10)->unsigned(),
```
其次，现在可以使用表达式作为默认值：

```
$this->integer()->defaultExpression('CURRENT_TIMESTAMP');
```

## 控制台迁移生成器

./yii migrate/create 变得更灵活。现在基于迁移名字可以为迁移产生样板代码，所以你不需要输入那么多：

将会产生 `./yii migrate/create create_post --fields=title:string,body:text`：

```
class m150811_220037_create_post extends Migration
{
    public function up()
    {
        $this->createTable('post', [
            'id' => $this->primaryKey(),
            'title' => $this->string(),
            'body' => $this->text()
        ]);
    }
 
    public function down()
    {
        $this->dropTable('post');
    }
}
```
[请参阅指南来学习语法](http://www.yiiframework.com/doc-2.0/guide-db-migrations.html#generating-migrations)。我们希望它能为你节省更多的时间。

## RBAC 扩展接口

RBAC 接口扩展了 [`getUserIdsByRole()`](http://www.yiiframework.com/doc-2.0/yii-rbac-managerinterface.html#getUserIdsByRole()-detail) 方法，在实现你自己的管理角色和权限的UI时，用这个方法比较方便。

错误处理和 dumping

* Yii 改善 JSON 错误处理来支持 PHP 5.5 错误代码，有助于确定为什么编码失败。
* `VarDumper::dump()` 现在兼容 `__debugInfo()` PHP 魔术方法。
* 出于安全因素，现在错误处理在默认的错误页面不显示 `$_ENV` 和 `$_SERVER`。可以通过 [`yii\web\ErrorHandler::$displayVars`](http://www.yiiframework.com/doc-2.0/yii-web-errorhandler.html#$displayVars-detail) 来设定要显示什么。
* `yii\helpers\VarDumper::export()` 能导出使日志和调试工具条更可靠的循环引用的对象。

## Security

我们不断地寻求最好的方法来得到真正的随机数，用于处理密码及与之相关的发展。现在安全组件使用 random_bytes()， LibreSSL，mcrypt，对 Windows 限制 OpenSSL 的使用并且比 crypt() 更好的选择是 password_hash()。

## PHP 7

ApcCache 现在可以恰当的处理 PHP 7 APCu 。为了使用它来设置 useApcu 缓存的属性值为 true。

## 内置 Web 服务器

现在你可以不用安装全部功能的 web 服务器来开发 Yii 2.0 apps，例如 nginx 或者 Apache，通过在控制台输入 `./yii serve` 并且打开你的浏览器输入 `http://localhost:8080`。主机名和端口可以通过指定额外的参数的类型 `./yii help` 查看语法细节。

## PJAX

全部的 PJAX 体验将非常的引人注目。在 PJAX 里有一些修复关于 data- 方法，重定向，加载脚本等。

## 验证

* 现在客户端的合法性验证会跳过 disabled 的输入项（禁用的输入项）。
* "compare" 验证器改进了错误信息。
* 现在可以在验证器指定范围内使用匿名函数。
* 现在文件验证器可以将 maxFiles 设置为 0 来允许无限数量的文件。

## Events

事件现在考虑了继承，可以被整个 类/接口 层次绑定。[详细信息请参阅指南](http://www.yiiframework.com/doc-2.0/guide-concept-events.html#interface-level-event-handlers)。

## Assets

现在在 asset 捆绑类(AssetBundle)中可以为每个 CSS 或者 JavaScript 文件指定属性：

```
public $css = [
    'default_options.css',
    ['tv.css', 'media' => 'tv'],
    ['screen_and_print.css', 'media' => 'screen, print']
];
 
public $js = [
    'normal.js',
    ['defered.js', 'defer' => true],
];
```
## REST API

以调试为目的，可以配置 JsonResponseFormatter，用于格式化要返回的JSON数据，使之更具可读性。您可以按以下方式配置响应应用程序组件：

```
'response' => [
    // ...
    'formatters' => [
        \yii\web\Response::FORMAT_JSON => [
             'class' => 'yii\web\JsonResponseFormatter',
             'prettyPrint' => YII_DEBUG, // use "pretty" output in debug mode
             // ...
        ],
    ],
],
```