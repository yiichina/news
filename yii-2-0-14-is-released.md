我们很高兴的宣布 Yii 框架 2.0.14 版本发布了。请参考说明 http://www.yiiframework.com/download/ 安装或升级到此版本。

 2.0.14 版本版本是 Yii 2.0 的较小的发行版，它包含了超过 100 处的增强和 bug 修复。 包括安全补丁。这也是Yii 2.0 包含增强功能的最后一个版本。 This means that we will focus on including new features into the 2.1.x branch and 2.0.x will only receive bug fixes from now on. We will make an announcement on the time frames for supporting different branches with the release of version 2.1.

局部微小的改动将会影响您现有的应用程序，所以请务必检查 UPGRADE.md 文件。

感谢贡献于框架的 Yii 团队的所有成员。 We did it together!

您可以通过 starring 或者 watching Yii 2.0 GitHub 项目来跟进 Yii 2 的开发进度。有很多活跃的 Yii 社区，如果你需要帮助或者想要分享你的经验,，欢迎加入他们。

Since there is Yii 2.1 in development now, make sure you have a version constraint in your composer.json, that does not allow it to be installed automatically on update, so when next major version of Yii is released, your project won't break by itself. A version constraint that does not include 2.1 is for example ~2.0.14, make sure you do not have >= or * in version constraints in composer.json.

Below we summarize some of the most important features/fixes included in this release. A complete list of changes can be found in the CHANGELOG.

Scalability and concurrency
While not important at early project stages, scalability and concurrency issues are major obstacles for business growth. In this release we have identified and fixed a concurrency issue when writing values and regenerating IDs in yii\web\DbSession and three issues that may appear in master-slave setups while using yii\web\DbSession, yii\validators\UniqueValidator and yii\validators\ExistValidator.

Validator enhancements
Additionally to what's mentioned in previous section, there are enhancements in built-in validators.

First, the ExistValidator is now able to check relations when its new targetRelation property is set. That basically means the following rules definition is now possible:

```
public function rules()
{
    return [
        [['customer_id'], 'exists', 'targetRelation' => 'customer'],
    ];
}
 
public function getCustomer()
{
    return $this->hasOne(Customer::class, ['id' => 'customer_id']);
}
```
Another enhancement is about FileValidator. It got a new property called minFiles to specify the minimum number of files a user must upload.

Behaviors
yii\behaviors\BlameableBehavior got a defaultValue property that is used in case user ID could not be determined. That is often the case when Active Record model is used within console application.

New yii\behaviors\AttributeTypecastBehavior property typecastAfterSave could be set to true to make values typecasted after model is saved. That makes attribute types consistent if they're casted when saved to database.

New yii\behaviors\CacheableWidgetBehavior was added. It automatically caches widget contents according to duration and dependencies specified and could be added to a widget like the following:

use yii\behaviors\CacheableWidgetBehavior;
 
```
public function behaviors()
{
  return [
      [
          'class' => CacheableWidgetBehavior::className(),
          'cacheDuration' => 0,
          'cacheDependency' => [
              'class' => 'yii\caching\DbDependency',
              'sql' => 'SELECT MAX(updated_at) FROM posts',
          ],
      ],
  ];
}
```
Databases and ActiveRecord
This release adds lots of great things related to databases and ActiveRecord that were worked by Dmitry Naumenko, Sergey Makinen, Robert Korulczyk, Nikolay Oleynikov and other community members.

Custom data types and object conditions
Custom data types support was implemented. Added JSON support for MySQL and PostgreSQL and arrays support for PostgreSQL. In order to do that, Query Builder internals were refactored significantly and now support object format for conditions:

```
$query->andWhere(new OrCondition([
    new InCondition('type', 'in', $types),
    ['like', 'name', '%good%'],
    'disabled=false',
]));
```
There are two good things about it. First, it's easier to support existing conditions and add new ones for Yii team. Additionally to conditions for JSON and arrays, it already resulted in a new BetweenColumnsCondition. There could be more added in Yii 2.1. Second, it's convenient to add custom conditions now.

Query Builder flexibility
Query Builder flexibility was improved overall. It is now possible to pass yii\db\Query anywhere, where yii\db\Expression was supported. For example, it is now possible to use it like the following:

```
$subquery = (new Query)
    ->select([new Expression(1)])
    ->from('tree')
    ->where(['parent_id' => 1, 'id' => new Expression('tree.parent_id']));
 
(new Query())
    ->from('tree')
    ->where(['or', 'parent_id = 1', $subquery])
```

Upserts
Another big thing is upserts for all databases Yii database layer supports. Upsert is an atomic operation that inserts rows into a database table if they do not already exist (matching unique constraints), or update them if they do:

```
Yii::$app->db->createCommand()->upsert('pages', [
    'name' => 'Front page',
    'url' => 'http://example.com/', // URL is unique
    'visits' => 0,
], [
    'visits' => new \yii\db\Expression('visits + 1'),
], $params)->execute();
```
Will either insert a new page record or increment its visit counter atomically.

Schema builder and migrations
Schema builder now supports tiny integer and JSON so you can use the following in migrations:

```
$this->createTable('post', [
    'id' => $this->primaryKey(),
    'text' => $this->text(),
    'title' => $this->string()->notNull(),
    'attributes' => $this->json(),
    'status' => $this->tinyInteger(),
]);
```
Another enhancement about migrations is the ability to create and drop database views:

```
$this->createView(
    'top_10_posts',
    (new \yii\db\Query())
        ->from('post')
        ->orderBy(['rating' => SORT_DESC])
        ->limit(10)
);
 
$this->dropView('top_10_posts');
```
New query caching syntax
It was possible before to wrap DB calls with connection's cache method. Now there are handy shortcuts:

```
// at query level
(new Query())->cache(7200)->all();
 
// at AR level
User::find()->cache(7200)->all();
Active Record relations
Active Record now resets related models after corresponding attribute updates:

$item = Item::findOne(1);
echo $item->category_id; // 1
echo $item->category->name; // weapons
 
$item->category_id = 2;
echo $item->category->name; // toys
```
Error handling
Log targets now throw exception in case log can't be exported properly. Previously they were failing silently.

Another case where Yii is now throwing yii\web\HeadersAlreadySentException exception instead of being silent if headers were already sent before (thus, it's not possible to send more).

It is now possible to configure Yii error handler via setting $traceLine property to generate links in the exception code so these could be opened directly in IDE. Configuration is similar to debug toolbar:
```
'components' => [
    // ...
    'errorHandler' => [
        'errorAction' => 'site/error',
        'traceLine' => '<a href="ide://open?url={file}&line={line}">{html}</a>',
    ],
],
```
Using yii\web\ErrorAction::$layout property you can set layout from error action config conveniently:
```
class SiteController extends Controller
{
    // ...
    /**
     * @inheritdoc
     */
    public function actions()
    {
        return [
            'error' => [
                'class' => 'yii\web\ErrorAction',
                'layout' => 'error', // <-- HERE
        ],
    ];
}
```

Security
There are two vulnerabilities discovered and fixed in this release:

CVE-2018-6009. The switchIdentity() function in web/User.php did not regenerate the CSRF token upon a change of identity.
CVE-2018-6010. Remote attackers could obtain potentially sensitive information from exception messages printed by the error handler in non-debug mode.
PHP 7.2
This release brings full PHP 7.2 compatibility. We have adjusted yii\filters\HttpCache, FileHelper::getExtensionsByMimeType() and yii\web\Session to work well with all PHP versions we support.

Widgets, forms and clientside
Markup generated for <script> tag doesn't have type attribute anymore. Additionally to better looking, it makes HTML5 markup validator happy.

In form active fields (for both Active Form and Html helper) you can now automatically include placeholder that matches field's attribute:

```
<?=  Html::activeTextInput($post, 'title', ['placeholder' => true]) ?>
Another thing added to ActiveForm is an ability to choose which HTML element receives validation state classes:

<?php $form = ActiveForm::begin([
    'validationStateOn' => ActiveForm::VALIDATION_STATE_ON_INPUT,
]) ?>
```
That possibly allows Yii bootstrap extension to adopt Bootstrap 4.

It's now possible to register JavaScript variables via PHP code:

```
class SiteController extends Controller
{
    public function actionIndex()
    {
        $this->view->registerJsVar('username', 'SilverFire');
        return $this->render('index');
    }
}
```
While it is widely used method of passing data from PHP to JavaScript, before using it consider HTML5 data attributes.

Events
Paul Klimov added wildcard matching to events so it's now possible to subscribe to multiple objects or class events that match the pattern. That is very useful for logging and audit. Brand new section in the guide is full of examples using the feature.

APIs, serializers and filters
When configuring JsonResponseFormatter you can now specify content type:

```
'components' => [
    'response' => [
        // ...
        'formatters' => [
            \yii\web\Response::FORMAT_JSON => [
                'class' => \yii\web\JsonResponseFormatter::className(),
                'contentType' => \yii\web\JsonResponseFormatter::CONTENT_TYPE_HAL_JSON,
            ],
        ],
    ],
]
```
Data filter can now handle lt,gt,lte and gte on yii\validators\DateValidator.

yii\base\ArrayableTrait::toArray() now allows recursive $fields and $expand. That means REST APIs queries expand could be specified now as extra1.extra2 and that would expand extra1 in the original data set and then extra2 in extra1 data set i.e. queries like http://localhost/comments?expand=post.author are possible.

In case you need to convert model validation errors into JSON you may use \yii\helpers\Json::errorSummary() now.

Custom authentication headers are now easier to set up thanks to added yii\filters\auth\HttpHeaderAuth.

Console
How many times you wished there was a built-in method to print model validation errors into console instead of doing foreach? Now there is one:

```
if (!$model->validate()) {
    echo "Model is not valid:\n";
    echo \yii\helpers\Console::errorSummary($model);
    return ExitCode::DATAERR;
}
```
bash and zsh command completion got better. Now it understands ./yii help.

When invoking a command options could now be specified as both camelCase and kebab-case i.e. --selfUpdate and --self-update. Moreover, in addition to --<option>=<value> console optios syntax, it's now possible to use --<option> <value> syntax.

Routing
Short verb syntax could now be used in URL rule groups:

```
'components' => [
    'urlManager' => [
        // ...
        'rules' => [
            new GroupUrlRule([
                'prefix' => 'file',
                'rules' => [
                    'POST document' => 'document/create',
                ],
            ]),
    ],
],
```

i18n
New yii\i18n\Locale component was added having getCurrencySymbol() method that is able to get currency symbol for a given locale.

Helpers
This release brings very interesting enhancements to Yii helpers.

yii\helpers\FileHelper got two new methods. findDirectories() returns the directories found under the specified directory and subdirectories. It is similar to existing findFiles() but works with directories. unlink() removes a file or symlink in a cross-platform way which proved to be tricky.

yii\helpers\StringHelper got a new matchWildcard() method that does the same as native fnmatch() but does it consistently among different OS. Native one proved to differ from system to system.

yii\helpers\IpHelper was added. It allows determining IP version by address, comparing address against a mask or range, and expanding IPv6. Usage is simple and convenient:

```
if (!IpHelper::inRange($ip, '192.168.1.0/24')) {
    // deny access
}
```
DI container
Container got an ability to reuse definitions as properties:

```
'container' => [
    'definitions' => [
        \console\models\TestService::class => [
            'class' => \console\models\TestService::class,
            'model' => Instance::of(\console\models\TestModel::class)
        ],
        \console\models\TestModel::class => [
            'class' => \console\models\TestModel::class,
            'property' => 20,
        ],
    ],
],
```
In the code above the model property of TestService will be set with an instance of TestModel class configured in the container.

Project templates
Additionally to minor adjustments basic project template got Docker and vagrant support.

Preparing for 2.1
Brandon Kelly proposed a very good idea to mark some methods and classes that were already removed in 2.1 as deprecated. That should make transition from 2.0 to 2.1 easier:

Deprecated yii\base\BaseObject::className() in favor of native PHP syntax ::class, which does not trigger autoloading (only works with PHP >=5.5).
Deprecated XCache and Zend data cache support as caching backends.
Deprecated yii\BaseYii::powered() method.
Added yii\base\InvalidArgumentException and deprecated yii\base\InvalidParamException.
Added yii\BaseYii::debug() and deprecated yii\BaseYii::trace().
Code using these methods would work as usual except that IDEs will mark it deprecated.
