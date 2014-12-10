# Yii 2.0 RC 发布了 #

我们很高兴地宣布Yii2.0 的 RC（发布候选）发布。你可以遵循 [yiiframework.com](http://www.yiiframework.com) 说明安装或升级到这个版本。

此RC版本包括大约100个bug修复和200个新功能和功能改进。这是因为现有 [Beta版本](http://www.yiiframework.com/news/77/yii-2-0-beta-is-released/) 5个月集约化开发的结果。在此期间，我们从Yii的社区用户获得了很多帮助。在此，我们感谢[为Yii贡献](https://github.com/yiisoft/yii2/graphs/contributors)的每一个人，才促使此版本发布。你们是最棒的！

## 常见问题 ##

- **2.0 RC是什么意思？** RC是指候选发布版。这是GA（一般情况）发布之前的最后一个开发版本。对我们来说，针对GA版本剩下的工作主要有小问题，修复和文档。

- **2.0 GA什么时候发布？** 这取决于我们收到这个RC版本的反馈。我们有一个初步的计划，将发布2.0 GA在两周左右，如果RC版本被证明是足够稳定。

- **我可以将RC应用于我的项目吗？** 是的，我们强烈建议您尝试一下，在你的新项目，并给我们反馈一下。由于2.0 GA是指日可待，我们建议您不要使用RC在生产中使用，因为我们仍然可能引入重大更改，尽管这种可能性是非常小的。

- **是否有 Yii2.0 的文档？** 是的，[权威指南](http://www.yiiframework.com/doc-2.0/guide-README.html)，现在是Yii最全面，最深入的教程，以及[API文档](http://www.yiiframework.com/doc-2.0/)，你会用它来查找在这个框架中各个类的使用并参考。

- **如何升级我用Yii1.1写的应用程序到Yii2.0？** 请参考[从Yii1.1升级](http://www.yiiframework.com/doc-2.0/guide-upgrade-from-v1.html)。请注意，由于2.0是1.1的完全重写，升级不会是一个小工程。如果在1.1的应用程序已经运行稳定，我们建议您继续使用1.1，除非你有足够的时间和资源做升级。

- **我应该怎样从2.0Beta或Alpha升级？** 请按照[升级说明](https://github.com/yiisoft/yii2/blob/2.0.0-rc/framework/UPGRADE.md)

- **我怎样使用2.0的开发？** YII2.0的所有开发活动都发生在GitHub上：https://github.com/yiisoft/yii2 。您可以设置提醒或关注这个项目获得开发更新。您也可以按照我们的Twitter更新在https://twitter.com/yiiframework 或 加入我们的[Facebook群组](https://www.facebook.com/groups/yiitalk/)。


## 2.0 RC的重大改进 ##

在此版本中，我们已经包含了许多有用的功能和改进。下面我们总结了一些最重要的。查看这个版本所有变化的完整列表，可以在[更新日志](https://github.com/yiisoft/yii2/blob/2.0.0-rc/framework/CHANGELOG.md)中找到。如果你想了解你可以用Yii 2.0 一般做什么，请阅读[权威指南](http://www.yiiframework.com/doc-2.0/guide-README.html)。

### 安全 ###

一些安全专家，其中包括 [汤姆·沃斯特](https://github.com/tom--) 和 [安东尼·费拉拉](https://github.com/ircmaxell)，帮助审查YII关于它的安全性方面代码，并留下关于如何提高Yii的安全许多重要的反馈。汤姆甚至帮我们改写了一些安全代码，这会导致更好的密钥生成和加密，防止计时攻击，以及许多其他的东西。

为了支持一些安全功能的定制，我们已经把以前的安全辅助类移到安全应用程序组件。这样一来，就可以通过这种方式使用 
`Yii::$app->security->encrypt()`。

我们还提出了一些其他小但重要的变化，进一步提高Yii的安全性。例如，httpOnly现在默认打开所有的cookie; 如果你设置`yii\web\Request::enableCsrfCookie`为false，CSRF令牌可以存储在会话，而不是cookie。

### 数据库 ###

#### 数据库复制和读写分离 ####

Yii 现在已经内置了支持数据库复制和读写分裂。随着数据库复制，数据是从所谓的主服务器从服务器复制。所有的写操作和更新必须在主服务器上，读取时可能会发生在从服务器上。要使用此功能，只需设置类似如下的数据库连接：

```php
[
    'class' => 'yii\db\Connection',
 
    // configuration for the master
    'dsn' => 'dsn for master server',
    'username' => 'master',
    'password' => '',
 
    // common configuration for slaves
    'slaveConfig' => [
        'username' => 'slave',
        'password' => '',
    ],
 
    // list of slave configurations
    'slaves' => [
        ['dsn' => 'dsn for slave server 1'],
        ['dsn' => 'dsn for slave server 2'],
        ['dsn' => 'dsn for slave server 3'],
    ],
]
```

使用这个配置，你可以继续写数据库查询代码像往常一样。如果一个查询从数据库读取数据，一个从数据库将被用于自动执行查询（一个简单的负载平衡算法的实现对从数据库的选择）；如果这个查询是更新或插入到数据库，主数据库会被使用。

#### 事务 ####

这里说一下关于使用数据库事务几项功能的改进。

首先，你现在可以像下面这样以回调形式的事务工作：

```php
$connection->transaction(function() {
    $order = new Order($customer);
    $order->save();
    $order->addItems($items);
});
```

这相当于下列冗长的代码：

```php
$transaction = $connection->beginTransaction();
try {
    $order = new Order($customer);
    $order->save();
    $order->addItems($items);
    $transaction->commit();
} catch (\Exception $e) {
    $transaction->rollBack();
    throw $e;
}
```

其它，事务可以触发一系列事件。 例如，当你开始一个新的事务时，beginTransaction事件被一个数据库连接触发；当这个事务成功提交时，commitTransaction事件触发。您在使用事务处理时可以响应这些事件执行一些预处理和后处理任务。 

最后，开始一个新的事务时，您可以设置事务隔离级别（例如READ COMMITTED）。例如，

```php
$transaction = $connection->beginTransaction(Transaction::READ_COMMITTED);
```


#### 建立查询条件 ####

建立一个查询条件时，可以使用任意运算符。在下面这个例子中， 运算符 >=用于建立查询条件的年龄 >= 30。Yii 会正确引用的列名称，并使用参数绑定到处理的值。

```php
$query = new \yii\db\Query;
$query->where(['>=', 'age', 30]);
```

当您建立一个in或者not条件时，您可以使用子查询，如下所示：

```php
$subquery = (new \yii\db\Query)
    ->select('id')
    ->from('user')
    ->where(['>=', 'age', 30]);

 
// fetch orders that are placed by customers who are older than 30  
$orders = (new \yii\db\Query)
    ->from('order')
    ->where(['in', 'customer_id', $subquery])
    ->all();
```


### Asset 管理 ###

Yii 包括了Bower和NPM包。它采用优秀的Composer Assets 插件通过 Composer 接口管理依赖于Bower/NPM包（例如 jQuery, jQuery UI, Bootstrap）。

由于这个变化，在您开始安装或升级到Yii 2.0 RC时，您必须安装插件运行以下命令（一次性的）。

`php composer.phar global require "fxp/composer-asset-plugin:1.0.0-beta2"`

现在，如果你运行下面的命令，你就可以安装vendor目录下的jQuery Bower：

`php composer.phar require bower-asset/jquery:2.1.*`

请参考 权威指南 关于 assets 获取更多关于一般asset管理的详情。

### 数据格式 ###

我们做的数据格式化类和先前的yii\base\Formatter和yii\i18n\Formatter类显著重构成一个单一的yii\i18n\Formatter。
新的格式化类提供了一个一致的接口，不管PHP的intl扩展安装与否。如果没有安装此扩展，它将很好的回退到支持的没有国际化数据格式。

我们还统一指定日期和时间的方式是使用ICU格式。一些类比如DateValidator和JUI DatePicker 在默认情况下都合使用这种格式。
你可以，但是，仍然使用PHP日期格式的PHP prefxing 吧。例如，

```php
$formatter = Yii::$app->formatter;
$value = time();
echo $formatter->asDate($value, 'MM/dd/yyyy'); // same as date('m/d/Y', $value)
echo $formatter->asDate($value, 'php:Y/m/d');  // same as date('Y/m/d', $value)
echo $formatter->asDate($value, 'long');       // same as date('F j, Y', $value)
```

### 表单 ###

为ActiveForm的JavaScript代码的一些改进。

使用一组事件被触发，而不是使用回调在客户端验证过程中注入代码。你可以很容易地编写JavaScript代码来响应这些事件。例如，

```javascript
$('#myform').on('beforeValidate', function (event, messages, deferreds) {
    // called before validating the whole form when the submit button is clicked
    // You may do some custom validation here
});

$('#myform').on('beforeSubmit', function () {
    // called after all validations pass and the form should be submitted
    // You may perform AJAX form submission here. Make sure you return false to prevent the default form submission.
});
```

Deferred 验证也支持了。在上面的例子中，为beforeValidate事件deferreds参数允许你添加新的Deferred对象。利用该defeffed验证的支持，FileValidator和ImageValidator现在都支持客户端验证。

Several methods in the ActiveForm JavaScript code are now exposed so that you can more easily build dynamic forms on the client and support validation of input fields that are dynamically created. For example, the following JavaScript can be used to add validation support for a newly created input field "address":

```javascript
$('#myform').yiiActiveForm('add', {
    'id': 'address',
    'name': 'address',
    'container': '.field-address',
    'input': '#address',
    'error': '.field-address .help-block'
});
```

### 日志和错误处理 ###

您现在可以使用数组或对象的日志信息。默认的日志对象会自动将其转换成文字显示;而你的自定义日志目标类可以专门处理这些复杂的数据。

InvalidCallException，InvalidParamException，UnknownMethodException从现在的SPL BadMethodCallException延伸，使异常层次结构更加合理。

异常以显示堆栈跟踪方法类改进了显示方式。

### 开发工具 ###

Yii 调试器是一个非常有用的工具，在Yii应用运行时告诉你详细的调试信息。我们已经添加了一个新的调试器面板显示加载的资源包和它们的内容。

Yii 代码生成器Gii现在可以运行一个控制台命令！以前它只是提供一个Web界面，它虽然很直观，易于使用，但不会被一些铁杆用户喜爱。现在，每个人觉得它都是很高兴的。更重要的是，你可以创建一个新的Gii生成器像以前一样，它可以用在Web和控制台模式，无需作何额外的工作。

尝试在控制台模式下的GII，运行下面的命令：

```
# change path to your application's base path
cd path/to/AppBasePath

# show help information about Gii
yii help gii

# show help information about the model generator in Gii
yii help gii/model

# generate City model from city table
yii gii/model --tableName=city --modelClass=City
```

### 行为 ###

我们增加了一个新的行为 yii\behaviors\SluggableBehavior可以填充指定的模型属性的音译和调整后的版本，以便它可以在URL中使用。您可以使用这个行为就像如下，

```php
use yii\behaviors\SluggableBehavior;
 
public function behaviors()
{
   return [
       [
           'class' => SluggableBehavior::className(),
           'attribute' => 'title',
           // 'slugAttribute' => 'alias',   // store slug in "alias" column
           // 'ensureUnique' => true,       // ensure generation of unique slugs
        ],
    ];
}
```

行为现在可以指定和使用匿名。例如，

```php
$component->attachBehaviors([
    'myBehavior1' => new MyBehavior,  // a named behavior
    MyBehavior::className(),          // an anonymous behavior
]);
```

### 模板引擎 ###

无论是Smarty还是Twig视图渲染已经得到显著改善。特殊语法引入了许多Yii的概念，而我们收到的反馈意见，这些新的语法允许人们使用Smarty和Twig作为普通的PHP模板使其提高工作效率。要了解更多关于新语法，请参见权威指南。 