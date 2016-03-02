我们很高兴地宣布 Yii Framework 2.0.6 版本发布了。请参考说明 http://www.yiiframework.com/download/ 安装或升级到此版本。

2.0.6 版本是 Yii 2.0 的一个补丁发布，其中包含超过70个小的新功能和bug修复以及对文档和和指导翻译的许多改进。

我们感谢我们的优秀 Yii 社区帮助我们 pull request 和有价值的讨论。谢谢！

你可以通过 starring 或者 watching Yii 2.0 GitHub 项目的方式来跟踪 Yii 2 的开发进度。你可以通 Yii 的官方微博或者加入 Yii Facebook group 来联系其他的 Yii 开发者。

下面我们总结了一些包含在这个版本最重要的特性。

## 更好的迁移语法

我们计划为 2.1 初期支持结构生成器，但是 pana1990 和 vaseninm 提前把计划的任务完成了， 现在我们拥有了更好的迁移语法：

```
$this->createTable('example_table', [
    'id' => $this->primaryKey(),
    'name' => $this->string(64)->notNull(),
    'type' => $this->integer()->notNull()->defaultValue(10),
    'description' => $this->text(),
    'rule_name' => $this->string(64),
    'data' => $this->text(),
    'created_at' => $this->datetime()->notNull(),
    'updated_at' => $this->datetime(),
]);
```

这个新的迁移语法对编写数据库无关的迁移是非常有用的。当然，传统的语法仍然有效。

## 错误处理

在这个版本中有许多修正和改进，这使得错误报告和显示错误更加
可靠和有用：

- Yii 现在能准确的处理 HHVM 致命错误。
- 警告错误写入日志以防 FileCache 失败导致写入文件中。
- yii\web\ErrorAction displays 404 error instead of blank page on direct access.
- Json::encode() and Json::decode() are handling errors better throwing meaningful exceptions.
- ErrorHandler::logException() now logs the whole exception object instead of only its string representation. This can be used in custom log targets to provide more detailed error information.

## 让JavaScript更好的操控ActiveForm

使用 JavaScript 可以更加灵活操作ActiveForm。

您可以更新某些字段的错误信息：

```
// add error
$('#contact-form').yiiActiveForm('updateAttribute', 'contactform-subject', ["I have an error..."]);
 
// remove error
$('#contact-form').yiiActiveForm('updateAttribute', 'contactform-subject', '');
```

Or update the whole form and, optionally, summary at once:

```
$('#contact-form').yiiActiveForm('updateMessages', {
    'contactform-subject': ['Really?'],
    'contactform-email': ['I don\'t like it!']
}, 'There are errors!');
```

## yii 信息命令的改进

Message extraction command is now supports .pot file creation.

按如下方式嵌套 Yii::t() 也可以：

```
Yii::t('app', 'There are new {messages} for you!', [
    'messages' => Html::a(Yii::t('app', 'messages'), ['user/notificaitons']),
]);
```

It's always sorting created messages, even if there is no new one, while saving to PHP files. It allows you to have much smaller diff.

There's new config option called markUnused that allows configuring behaviour of adding @@ to unused messages.

## 资源

It's now possible to fine-tune what's published and what's not:

```
class MyAsset extends AssetBundle
{
    public $sourcePath = '@app/assets/js';
 
    public $js = [
        'app.js',
    ];
 
    public $depends = [
        'yii\web\YiiAsset',
    ];
 
    public $publishOptions = [
        'except' => '*.ts', // exclude TypeScript sources
        // 'only' => '*.js', // include JavaScript only
    ];
}
```

You can now customize the way directory name hashes (the ones in web/assets directory) are generated. It could be done right from application config:

```
return [
    // ...
    'components' => [
        'assetManager' => [
            'hashCallback' => function ($path) {
                return hash('md4', $path);
            }
        ],
    ],
];
```

## 额外的会话字段

You can now easily store extra data in session storage. Currently it's supported in yii\web\DbSession but could be possibly extended to more storages in future. In order to configure it, modify the session component in your application config:

```
return [
    // ...
    'components' => [
        'session' => [
            'class' => 'yii\web\DbSession',
            'readCallback => function ($session) {
                return [
                    'expireDate' => Yii::$app->formatter->asDate($fields['expire']),
                ];
            },
            'writeCallback' => function ($session) {
                return [
                    'user_id' => Yii::$app->user->id,
                    'ip' => $_SERVER['REMOTE_ADDR'],
                    'is_trusted' => $session->get('is_trusted', false),
                ];
            }
        ],
    ],
];
```