**Yii 2.0.8 发布了**

我们很高兴地宣布 Yii Framework 2.0.8 版本发布了。请参考说明 http://www.yiiframework.com/download/ 安装或升级到此版本。

2.0.8 版本是 Yii 2.0 的一个补丁发布，它包含了我们 [64 位开发者](https://github.com/yiisoft/yii2-framework/compare/2.0.7...2.0.8)的 300 次代码提交，对 150 个文件进行了修改，其中包含 [100 多个小的新功能和 bug 的修复](https://github.com/yiisoft/yii2/blob/2.0.8/framework/CHANGELOG.md)。

为了升级到新的版本，需要做一些额外的工作，请参阅 [UPGRADE.md](https://github.com/yiisoft/yii2/blob/2.0.8/framework/UPGRADE.md) 文件。

感谢我们的 [Yii 社区](https://github.com/yiisoft/yii2/graphs/contributors)，社区给我们提供了很多有价值的 pull requests 和 discussions。没有你就没有本次发布！

你可以通过 starring 或者 watching [Yii 2.0 GitHub](https://github.com/yiisoft/yii2) 项目来跟进 Yii 2 的开发进度。你也可以通过 Yii Twitter feeds 或者加入 Yii Facebook group 来联系其他的 Yii 开发成员。论坛中也有关于本次发布的新闻帖子。

下面我们总结了一些本次发布中的重要功能/修复。关于这个版本更新的完整列表，请参阅 [CHANGELOG](https://github.com/yiisoft/yii2/blob/2.0.8/framework/CHANGELOG.md)

## PHP 7 兼容

Yii 2.0.8 对于 PHP 7 有几个兼容性的补丁。一个是关于全局性的错误处理，另一个是关于 JSON 错误处理。可能大家都了解，PHP 7 在今年内会变得越来越普及，因为新的 Ubuntu LTS 版本已经包含了它。我们正在为这些即将到来的改变做准备。

## 数据库和 ActiveRecord

我们终于在 Yii 2 中见到了我们熟知的 Yii 1.1 的函数式编程。在 Yii 1.1 中，函数式编程体现在对 GridView 列的一些过滤操作符中。现在你可以使用 `yii\db\Query::andFilterCompare()` 方法来对你的 GridView 添加过滤操作符。目前还没有关于这部分的文档，contributions welcome!

一个新的 event 被添加到 ActiveRecord。现在，当一个记录被更新完毕的时候 ActiveRecord 会触发一个 EVENT_AFTER_REFRESH 事件。

## Schema 和迁移

在本次发布中，感谢我们的社区，在这个版本中我们得到了一系列关于数据库 Schema Builder 的增强。Schema Builder 通常用于数据库迁移，或者仅仅是为了别的原因修改数据库的结构。

首先，我们现在可以在数据表或者列中添加注释。在数据库迁移中，关于列的定义类似代码如下：

`'title' => $this->string()->notNull()->comment('Hello, I am the title!')`,
如果使用分开的方法的话代码如下：

```
$this->addCommentOnTable('user', 'This is a table comment.');
$this->addCommentOnColumn('user', 'name', 'This is a column comment.');
$this->dropCommentFromColumn('user', 'name');
$this->dropCommentFromTable('user');
```
接下来，你可以为你新添加的列指定位置：

```
$this->string()->notNull()->first();
$this->string()->notNull()->after('anotherColumn');
Unsigned primary keys are now easy to define via `$this->primaryKey()->unsigned()`.
```
`./yii migrate/create` 命令同样得到了一些增强。这个命令现在有了一个新的 `useTablePrefix` 选项，如果设置为 `true` 将导致生成的代码使用表前缀。

现在 支持使用 --fields （参数） 生成外键：

`yii migrate/create create_post --fields="author_id:integer:notNull:foreignKey(user),category_id:integer:defaultValue(1):foreignKey,title:string,body:text"`

## 表单和验证

你试过在实现文件上传的时候忘记设置合适的编码方式吗？现在 Yii 帮你完成了这些。如果你需要文件输入操作， Yii 对表单会自动生成必要的编码方式。

新版本中，指定验证规则的时候，你可以包含一个属性用来验证，如果你不希望将这个属性设置为安全属性用于块赋值，你可以在这个属性的前面添加一个感叹号（!）。

新版本中，在使用 `FileValidator` 的时候，你可以通过 `wildcards` 来指定 mimeTypes 。举个例子， `image/*` 可以匹配所有以 `image/` 开头的 mimetypes （例如 `image/jpeg` `image/png`）。

新的 `DateValidator` 将对时间值提供全功能的验证支持。这在以往的版本也是支持的，但是会有一些限制。现在你可以将 `DateValidator` 的 `$type` 属性设置为 `TYPE_DATETIME` 或者 `TYPE_TIME` 来开启时间的验证或者是完成国际化。

## 安全

感谢 Tom Worster 的研究和研究之后的讨论，安全组件得到了一些增强：

* It now avoids reading more bytes than needed from /dev/urandom and /dev/random.
* 在 FreeBSD 中建议使用 `/dev/random` 而不是 `/dev/urandom` 。
* 生成随机数的性能得到了增强。
这些都不是很严重的问题，所以没必要为了这个着急从 2.0.7 升级。

另外的一个引人注目的地方是 `Security` 组件得到了覆盖性更广的测试，这些测试在不同的环境下进行。

## 命令行

为已经存在的命令添加了一些可选参数，现在命令行控制器可以接受参数短语。例如，在创建数据库迁移的时候，你可以使用如下命令：

`./yii migrate/create -p=@app/modules/somemodule/migrations -t=module_migrations new_migration`

代替

`./yii migrate/create` --migrationPath=@app/modules/somemodule/migrations --migrationTable=module_migrations new_migration

你也可以在你的命令行控制器中覆盖 `yii\console\Controller::optionAliases()` 方法并指定命令短语。

## 为配置中的闭包进行依赖注入

在配置中的使用闭包所产生的依赖现在会被自动注入：

```
'components' => [
    'pheanstalk' => function(yii\web\User $user) {
        $result = new \Pheanstalk\Pheanstalk('localhost');
        $result->watch($user->getId());
        return $result;
    },
    // ...
```
## 基于 PostgreSQL 的互斥锁

现在有了 PostgreSQL 的互斥锁驱动，所以如果你在使用 PostgreSQL，你就可以有更多的选项来处理锁的问题。

## 高级应用

高级应用现在可以让你使用 vagrant 来安装一个独立的开发环境。详细信息请参阅[文档](https://github.com/yiisoft/yii2-app-advanced/blob/master/docs/guide/start-installation.md#installing-using-vagrant)。