# Yii 2.0.0 发布了

Yii 2.0终于来了，经过三年多的深入开发之后，在超过 [300位作者](https://github.com/yiisoft/yii2/graphs/contributors) 的近 [10000次提交](https://github.com/yiisoft/yii2/commits/master) 后。谢谢您的支持和耐心等候！

正如你已经了解到的，Yii 2.0 是在之前1.1版本基础上完全重写了。我们为了通过保持Yii 中原始的简单性和可扩展性，同时采用最新的技术和功能，使之成为最先进的PHP框架。今天，我们高兴的宣布，我们已经达到了我们的目标。

以下是有关Yii 和 Yii 2.0 的一些有用链接：

- [Yii 项目网站](http://www.yiiframework.com/)
- [Yii 2.0 GitHub Project](https://github.com/yiisoft/yii2)：你可以start或watch它跟踪Yii的开发活动。
- [Yii Facebook 群组](https://www.facebook.com/groups/yiitalk/)
- [Yii Twitter 订阅](https://twitter.com/yiiframework)
- [Yii LinkedIn 群组](https://www.linkedin.com/groups/yii-framework-1483367)

下面我们将总结一些这方面期待已久的版本亮点。你可以检出[入门](http://www.yiiframework.com/doc-2.0/guide-index.html#getting-started)部分，如果你冲动的想先尝试一下。

## 亮点

### 采用标准和最新技术

Yii 2.0 采用 PHP 的命名空间和特质，[PSR 标准](http://www.php-fig.org/psr/)，[Composer](https://getcomposer.org/)，[Bower](http://bower.io/) 和 [NPM](https://www.npmjs.org/)。所有这些使框架更加清爽并与其它类库实现相互操作。

### 坚实的基础类

类似 1.1，Yii 2.0 支持[对象属性](http://www.yiiframework.com/doc-2.0/guide-concept-properties.html)通过getter和setter[配置](http://www.yiiframework.com/doc-2.0/guide-concept-configurations.html)，[事件](http://www.yiiframework.com/doc-2.0/guide-concept-events.html)和[行为](http://www.yiiframework.com/doc-2.0/guide-concept-behaviors.html)定义的属性。新的实现更加高效和有表现力。例如，您可以编写以下代码来响应事件：

```php
$response = new yii\web\Response;
$response->on('beforeSend', function ($event) {
    // respond to the "beforeSend" event here
});
```

Yii 2.0 实现了[依赖注入容器](http://www.yiiframework.com/doc-2.0/guide-concept-di-container.html)和[服务定位器](http://www.yiiframework.com/doc-2.0/guide-concept-service-locator.html)。这使得用 Yii 构建的应用程序更个性化和可测试性。

### 开发工具

Yii 2.0 附带了一些开发工具，使开发者更容易工作。

在 [Yii 调试器](http://www.yiiframework.com/doc-2.0/guide-tool-debugger.html)中允许你检查应用程序内部的运行状态。它也可以用来做性能分析，找出应用程序中的性能瓶颈。

像 1.1，Yii 2.0 也提供了 Gii，一个[代码生成工具](http://www.yiiframework.com/doc-2.0/guide-tool-gii.html)，可以降低你一很大部分的开发时间。Gii 扩展性非常强，使您可以自定义或创建不同的代码生成器。Gii 提供Web接口和控制台接口，以适应不同用户的喜好。

Yii 1.1 的API文档已经收到很多积级的反馈。很多人表示愿意为他们创造一个类似的文件应用。Yii 2.0 实现了这个[文档生成器](https://github.com/yiisoft/yii2/tree/master/extensions/apidoc)。这个生成器支持Markdown语法，允许你在一个更为简洁的语法写文件。

### 安全

Yii 2.0 帮助你写出更安全的代码。它具有防止 SQL 注入, XSS 攻击, CSRF 攻击, cookie 窃取等。安全专家[Tom Worster](https://github.com/tom--)和 [Anthony Ferrara](https://github.com/ircmaxell) 甚至帮助我们检查并重写一些安全相关的代码。

### 数据库

数据库的工作从来都不简单。 Yii 2.0 支持[数据库迁移](http://www.yiiframework.com/doc-2.0/guide-db-migrations.html)，[数据访问对象（DAO）](http://www.yiiframework.com/doc-2.0/guide-db-dao.html)，[查询生成器](http://www.yiiframework.com/doc-2.0/guide-db-query-builder.html)和 [Active Record](http://www.yiiframework.com/doc-2.0/guide-db-active-record.html)。与 1.1 相比， Yii 2.0 提高 Active Record的性能，并结合语法通过查询构建器和 Active Record 查询数据。下面的代码演示了如何使用无论是查询生成器还是活动记录查询客户数据。正如你所看到的，这两种方法使用链式方法调用它类似于SQL语法。

```php
use yii\db\Query;
use app\models\Customer;
 
$customers = (new Query)->from('customer')
    ->where(['status' => Customer::STATUS_ACTIVE])
    ->orderBy('id')
    ->all();
 
$customers = Customer::find()
    ->where(['status' => Customer::STATUS_ACTIVE])
    ->orderBy('id')
    ->asArray();
    ->all();
```

下面的代码演示了如何使用 Active Record 进行关联查询：

```php
namespace app\models;
 
use app\models\Order;
use yii\db\ActiveRecord;
 
class Customer extends ActiveRecord
{
    public static function tableName()
    {
        return 'customer';
    }
 
    // defines a one-to-many relation with Order model
    public function getOrders()
    {
        return $this->hasMany(Order::className(), ['customer_id' => 'id']);
    }
}
 
// returns the customer whose id is 100
$customer = Customer::findOne(100);
// returns the orders for the customer
$orders = $customer->orders;
```

而下面的代码演示了如何更新客户记录。在后端，参数绑定用于防止SQL注入攻击，并且只修改列被保存到数据库。

```php
$customer = Customer::findOne(100);
$customer->address = '123 Anderson St';
$customer->save();  // executes SQL: UPDATE `customer` SET `address`='123 Anderson St' WHERE `id`=100
```

Yii 2.0 支持范围最广的数据库。除了传统的关系型数据库，Yii 2.0 增加了对Cubrid，ElasticSearch，Sphinx的支持。它还支持NoSQL数据库，其中包括Redis和MongoDB。更重要的是，同样的查询生成器和Active Record API，可用于所有这些数据库，这使得你在不同的数据库之间很容易的进行切换。并在使用Active Record的时候，你甚至可以将数据从不同的数据库操作（如MySQL和Redis之间）。

对于大型数据库和高性能要求的应用，Yii 2.0 还提供了内置支持的数据库复制和读写分离。

### RESTful APIs

用几行代码，Yii 2.0 让你快速建立一套符合最新协议功能齐全的基于RESTful的API。下面的示例显示了如何创建一个RESTful API 服务的用户数据。

首先，创建一个控制器类 app\controllers\UserController 并指定 app\models\User 为模型的类型服务：

```php
namespace app\controllers;
 
use yii\rest\ActiveController;
 
class UserController extends ActiveController
{
    public $modelClass = 'app\models\User';
}
```

然后，修改有关在应用程序配置，以满足用户的数据在漂亮网址的urlManager组件的配置：

```php
'urlManager' => [
    'enablePrettyUrl' => true,
    'enableStrictParsing' => true,
    'showScriptName' => false,
    'rules' => [
        ['class' => 'yii\rest\UrlRule', 'controller' => 'user'],
    ],
]
```

这就是你需要做的！你刚刚创建的 API 支持如下：

```
    GET /users: list all users page by page;
    HEAD /users: show the overview information of user listing;
    POST /users: create a new user;
    GET /users/123: return the details of the user 123;
    HEAD /users/123: show the overview information of user 123;
    PATCH /users/123 and PUT /users/123: update the user 123;
    DELETE /users/123: delete the user 123;
    OPTIONS /users: show the supported verbs regarding endpoint /users;
    OPTIONS /users/123: show the supported verbs regarding endpoint /users/123.
```

您可以访问您的 API 使用 curl 命令类似如下，

```
$ curl -i -H "Accept:application/json" "http://localhost/users"

HTTP/1.1 200 OK
Date: Sun, 02 Mar 2014 05:31:43 GMT
Server: Apache/2.2.26 (Unix) DAV/2 PHP/5.4.20 mod_ssl/2.2.26 OpenSSL/0.9.8y
X-Powered-By: PHP/5.4.20
X-Pagination-Total-Count: 1000
X-Pagination-Page-Count: 50
X-Pagination-Current-Page: 1
X-Pagination-Per-Page: 20
Link: <http://localhost/users?page=1>; rel=self, 
      <http://localhost/users?page=2>; rel=next, 
      <http://localhost/users?page=50>; rel=last
Transfer-Encoding: chunked
Content-Type: application/json; charset=UTF-8

[
    {
        "id": 1,
        ...
    },
    {
        "id": 2,
        ...
    },
    ...
]
```

### 缓存

像 1.1 一样，Yii 2.0 支持全部范围的缓存选项，从服务器端缓存，如果段缓存，查询缓存到客户端的HTTP缓存。他们对各种缓存驱动程序，包括APC，Memcache的，文件，数据库等都支持。

### 表单

在 1.1 中，您可以快速创建一个同时支持客户端和服务器端验证HTML的表单。在Yii 2.0，它更容易使用表单。下面的示例显示了如何创建一个登录表单。

首先创建一个LoginForm的模型来表示所收集的数据。在这个类中，你将列出应该被用来验证用户输入的规则。验证规则将在以后用于自动生成所需的客户端JavaScript验证逻辑。

```php
use yii\base\Model;
 
class LoginForm extends Model
{
    public $username;
    public $password;
 
    /**
     * @return array the validation rules.
     */
    public function rules()
    {
        return [
            // username and password are both required
            [['username', 'password'], 'required'],
            // password is validated by validatePassword()
            ['password', 'validatePassword'],
        ];
    }
 
    /**
     * Validates the password.
     * This method serves as the inline validation for password.
     */
    public function validatePassword()
    {
        $user = User::findByUsername($this->username);
        if (!$user || !$user->validatePassword($this->password)) {
            $this->addError('password', 'Incorrect username or password.');
        }
    }
}
```

然后创建一个登录表单视图代码：

```php
use yii\helpers\Html;
use yii\widgets\ActiveForm;
 
<?php $form = ActiveForm::begin() ?>
    <?= $form->field($model, 'username') ?>
    <?= $form->field($model, 'password')->passwordInput() ?>
    <?= Html::submitButton('Login') ?>
<? ActiveForm::end() ?>
```

### 身份验证和授权

和 1.1 一样，Yii 2.0 提供了对用户进行认证和授权的内置支持。它支持的功能如登录，注销，基于cookie和基于令牌的认证，访问控制过滤器和基于角色的访问控制（RBAC）。

Yii 2.0 还提供了通过外部认证机构身份验证的能力。它支持的OpenID，OAuth1和OAuth2协议。

### 小部件

Yii 2.0 附带了一组丰富的用户界面元素，称为窗口小部件，以帮助您快带构建交互式用户界面。它具有内置的Bootstrap部件和jQuery UI小部件的支持。它也提供了常用的小工具，如pagers，grid view，list view，detail，所有这些都使得Web应用程序开发的真正快捷和享受过程。例如，下面的几行代码，你可以创建一个俄语的全功能的jQuery UI的日期选择器：

```php
use yii\jui\DatePicker;
 
echo DatePicker::widget([
    'name' => 'date',
    'language' => 'ru',
    'dateFormat' => 'yyyy-MM-dd',
]);
```

### 辅助类

Yii 2.0 提供了许多有用的辅助类来简化一些常见的任务。例如，HTML 辅助类包括一套方法来创建不同的HTML标签而URL辅助类可以让你更轻松地创建各种URL，如下所示：

```php
use yii\helpers\Html;
use yii\helpers\Url;
 
// creates a checkbox list of countries
echo Html::checkboxList('country', 'USA', $countries);
 
// generates a URL like "/index?r=site/index&src=ref1#name"
echo Url::to(['site/index', 'src' => 'ref1', '#' => 'name']);
```

### 国际化

Yii 具有强力的国际化支持，因为它被用于世界各地。它支持消息翻译以及视图翻译。它还支持基于地区的复数形式和数据格式，符合 [ICU](http://icu-project.org/apiref/icu4c/classMessageFormat.html) 标准。例如，

```php
// message translation with date formatting
echo \Yii::t('app', 'Today is {0, date}', time());
 
// message translation with plural forms
echo \Yii::t('app', 'There {n, plural, =0{are no cats} =1{is one cat} other{are # cats}}!', ['n' => 0]);
```

### 模板引擎

Yii 2.0 使用 PHP 作为其默认的模板语言。它还支持 [Twig](http://twig.sensiolabs.org/) 和 [Smarty](http://www.smarty.net/) 通过[模板引擎扩展](http://www.yiiframework.com/doc-2.0/guide-tutorial-template-engines.html)。它也可以让你创建支持其它模板引擎的扩展。

### 测试

Yii 2.0 通过 [Codeception](http://codeception.com/) 和 [Faker](https://github.com/fzaninotto/Faker) 加强了测试支持。它还配备了一个固定的框架，使您可以更加灵活管理您的固定数据。

### 应用程序模板

为了进一步减少开发时间，Yii 发布了两个应用程序模板，每个都是一个全功能的Web应用程序。基本应用程序模板，可以作为一个起点，开发小而简单的网站，如公司的门户网站，个人网站。高级应用程序模板，更适合建设涉及多个层和一个大的开发团队的大型企业应用程序。

### 扩展

虽然 Yii 2.0 已经提供了许多强大的功能，有一个东西使 Yii 更强大的是它的扩展架构。扩展是专门设计在Yii应用中使用，并提供现成使用功能的可再发行的软件包。Yii 提供了许多内置功能，如邮件，Bootstrap。Yii 还拥有一个很大的用户提供的扩展库截至发稿时包含近1700个扩展。我们还发现有超过1300个Yii 相关的软件包在 [packagist.org](https://packagist.org/search/?q=yii) 上。

## 入门

要开始使用Yii 2.0，只需要运行下面的命令：

```
# install the composer-asset-plugin globally. This needs to be run only once.
php composer.phar global require "fxp/composer-asset-plugin:1.0.0-beta3"

# install the basic application template
php composer.phar create-project yiisoft/yii2-app-basic basic 2.0.0
```

使用上述命令假如你已经安装了[Composer](https://getcomposer.org/)。如果没有，请按照 [Composer安装说明](http://getcomposer.org/doc/00-intro.md#installation-nix)进行安装。

请注意，您可能会被提示在安装过程中输入您的GitHub的用户名和密码。这是正常的。只要输入它们并继续就可以。

通过上面的命令，您可以通过以下URL访问准备使用的Web应用程序 `http://localhost/basic/web/index.php `。

### 升级

如果您是从先前的 Yii 2.0 开发版本升级（如2.0.0-beta，2.0.0-rc），请按照[升级说明](https://github.com/yiisoft/yii2/blob/master/framework/UPGRADE.md)。

如果您是从 Yii 1.1 升级，我们要提醒你，它不会太顺利，主要是因为Yii 2.0是一个完全重写了，有许多语法的变化。然而，大部分Yii 的知识仍然适用于2.0。请仔细阅读[升级说明](http://www.yiiframework.com/doc-2.0/guide-intro-upgrade-from-v1.html)，了解在2.0中引入的重大变化。

### 文档

Yii 2.0 有一个权威指南以及一个类参考。这个权威指南已经被翻译为[多种语言](https://github.com/yiisoft/yii2/tree/master/docs)。

此外，还有一些关于Yii 2.0的书刚刚发布，或正在由著名作者 [拉里·厄尔曼](http://www.larryullman.com/) 写的。拉里 甚至花费了大量的时间帮助我们打磨权威指南。亚历山大·马卡罗夫正在协调有关的[Yii2.0社区提供的cookbook](https://github.com/samdark/yii2-cookbook)，跟随他受欢迎的Yii 1.1 cookbook。

### 信用

在此，我们感谢对 Yii [作出的贡献的成员](https://github.com/yiisoft/yii2/graphs/contributors)。您的支持和贡献是无价的！