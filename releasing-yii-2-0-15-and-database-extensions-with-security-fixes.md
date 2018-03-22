今天，我们将发布 Yii 2.0.x 的多个版本和官方扩展来解决安全问题。

这些补丁解决的问题存在于 `ActiveRecord` 快捷方法 `findOne()` 和 `findAll()` 中，如果输入没有准备好，可能会允许 SQL 注入。 我们认为这是 Yii 的一个安全问题，因为这些方法的文档没有包含明确的警告，表明有些情况下通过未经过滤的用户输入可能是危险的。 感谢 [analitic1983](https://github.com/analitic1983) 让我们意识到这个问题。

这个问题的性质并不完全存在于 Yii 框架中，而是取决于应用程序如何使用 Yii。 我们已经改变了 Yii 对此问题的最坏影响（SQL注入）使之更强壮，但应用程序可能仍然是脆弱的，并且在某些情况下，应用程序代码的更改是必需的。 作为一项安全措施，`findOne()` 和 `findAll()` 现在仅限于仅对 AR 属性进行过滤。 在下面，我们将更详细地解释问题，并显示哪些应用程序代码受到影响，以及升级时需要调整哪些内容。

关于这个问题的讨论，有一个[论坛话题](http://www.yiiframework.com/forum/index.php/topic/76661-releasing-yii-2015-and-database-extensions-with-security-fixes/)。

## 受影响的类，方法和 Compose 包的汇总

* `yii\db\ActiveRecord::findOne()` 和 `yii\db\ActiveRecord::findAll()` 在 `yiisoft/yii2` 参考 [CVE-2018-7269](https://nvd.nist.gov/vuln/detail/CVE-2018-7269)。 如果输入没有准备好，方法允许 [SQL 注入](https://en.wikipedia.org/wiki/SQL_injection)。 攻击者可能会执行任意 SQL 查询或绕过在查询级别应用的访问检查方法。
* `yii\redis\ActiveRecord::findOne()` 和 `yii\redis\ActiveRecord::findAll()` 在 `yiisoft/yii2-redis` 参考 [CVE-2018-8073](https://nvd.nist.gov/vuln/detail/CVE-2018-8073)。 方法允许在 redis 服务器 lua 脚本环境中远程执行代码。攻击者可能可能在 Redis 服务器上操纵数据。
* `yii\elasticsearch\ActiveRecord::findOne()` 和 `yii\elasticsearch\ActiveRecord::findAll()` 在 `yiisoft/yii2-elasticsearch` 参考 [CVE-2018-8074](https://nvd.nist.gov/vuln/detail/CVE-2018-8074)。方法可能允许注入不同于期望的搜索条件或导致来自弹性搜索服务器的错误响应。

## 我的应用程序是否受影响？

此漏洞影响 2.0.x 分支的所有版本。 在 Yii 2.0.15 中已经修复。 对于 2.0.15 以下的版本，我们发布了两个补丁版本 2.0.13.2 和 2.0.12.1，分别将修复应用于2.0.13.1 和
 2.0.12。 2.0.14的用户可以升级到2.0.15，此版本中没有其他更改。

## 不受影响的代码

方法 `findOne()` 和 `findAll()` 接受一个参数，它可以是标量或数组。 如果调用代码确保标量被传递或者客户端输入不能修改数组的结构，那么您的应用程序不会受到此问题的影响。 以下代码示例不受此问题影响（`findOne()` 所示的示例也适用于 `findAll()`）：

```
// yii\web\Controller 确保 $id 是标量
public function actionView($id)
{
    $model = Post::findOne($id);
    // ...
}

// casting to (int) or (string) ensures no array can be injected (an exception will be thrown so this is not a good practise)
$model = Post::findOne((int) Yii::$app->request->get('id'));

// explicitly specifying the colum to search, passing a scalar or array here will always result in finding a single record
$model = Post::findOne(['id' => Yii::$app->request->get('id')]);
```

## 受影响的代码

但是下面的代码很容易受到攻击，攻击者可以使用任意条件注入数组，甚至利用SQL注入：

```
$model = Post::findOne(Yii::$app->request->get('id'));
```

对于上述示例，此版本中提供的修补程序已修复 SQL 注入部分，但攻击者仍可以通过与主键搜索不同的条件搜索记录，并违反应用程序业务逻辑。 所以直接传递用户输入可能会导致问题，应该避免。

## 我如何升级？

如果您使用 Yii 2.0.14：

`composer require "yiisoft/yii2":"~2.0.15.0"`

如果您使用 Yii 2.0.13：

`composer require "yiisoft/yii2":"~2.0.13.2"`

如果您使用 Yii 2.0.12：

`composer require "yiisoft/yii2":"~2.0.12.1"`

如果您使用 yii2-redis 扩展：

`composer require "yiisoft/yii2-redis":"~2.0.8"`

如果您使用 yii2-elasticsearch 扩展：

`composer require "yiisoft/yii2-elasticsearch":"~2.0.5"`


## 仅升级不够！

升级 Yii 可解决 SQL 注入问题，但通常不会使 `findOne()` 和 `findAll()` 安全。 检查应用程序中 `findOne()` 和 `findAll()` 的所有用法。 另请注意，`where()` 和 `filterWhere()` 永远不会转义列名，所以如果您需要传递一个变量作为列名，请确保它是安全的。

