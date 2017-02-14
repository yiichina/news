我们非常荣幸地宣布立即可用的1.1.8 Yii框架版本。本次发布的版本中,我们包括超过80处新特点、改进和修复错误。

如果查看本此版本的所有更新,请参阅[更新日志](http://www.yiiframework.com/files/CHANGELOG-1.1.8.txt)和[新功能](http://www.yiiframework.com/doc/guide/changes)。如果你准备升级,从一个更老的版本,请参阅,1.1.8[升级的提示](http://www.yiiframework.com/files/UPGRADE-1.1.8.txt)。

在下面,我们简略地介绍一下这个版本最令人兴奋的新特点。谢谢你的支持!  

## 自定义URL规则类

为了处理比CUrlRule更复杂的URL（例如：URL的一部分以数据库表中的一些数据而决定），我们可以编写自定义的URL规则类并在CUrlManager中加入它们。下面我们显示的URL规则的配置，使用自定义URL规则类：

```
array(
    // a standard rule mapping '/login' to 'site/login', and so on
    '<action:(login|logout|about)>' => 'site/<action>',
 
    // a custom rule to handle '/Manufacturer/Model'
    array(
        'class' => 'application.components.CarUrlRule',
        'connectionID' => 'db',
    ),
 
    // a standard rule to handle 'post/update' and so on
    '<controller:\w+>/<action:\w+>' => '<controller>/<action>',
),
```

  
自定义的URL规则类继承自CBaseUrlRule，可以像下面这样做：

```
class CarUrlRule extends CBaseUrlRule
{
    public $connectionID = 'db';
 
    public function createUrl($manager,$route,$params,$ampersand)
    {
        if ($route==='car/index')
        {
            if (isset($params['manufacturer'], $params['model']))
                return $params['manufacturer'] . '/' . $params['model'];
            else if (isset($params['manufacturer']))
                return $params['manufacturer'];
        }
        return false;  // this rule does not apply
    }
 
    public function parseUrl($manager,$request,$pathInfo,$rawPathInfo)
    {
        if (preg_match('%^(\w+)(/(\w+))?$%', $pathInfo, $matches))
        {
            // check $matches[1] and $matches[3] to see
            // if they match a manufacturer and a model in the database
            // If so, set $_GET['manufacturer'] and/or $_GET['model']
            // and return 'car/index'
        }
        return false;  // this rule does not apply
    }
}
```
    
## 改进自动加载器

Yii提供了自动加载器，可以在需要的时候包含进来。它现在可以与其它第三方自动加载器发挥的更出色，让它们在前端或末端自动加载器链。此前，Yii的自动加载器必须在自动加载器链的末端，因此它是为Yii的核心类和外部类触发：

```
// By default autoloader is registered before the Yii autoloader
Yii::registerAutoloader($autoloader);Now you can avoid it by passing true as a second parameter:

// When second parameter $append is true, autoloader is being added after the Yii autoloader
Yii::registerAutoloader($autoloader, true);
```

默认情况下，Yii类自动加载工作依赖于PHP的include路径。然而，一些共享主机服务可能不允许设置PHP的include路径。如果你遇到这样的问题，现在可以在应用程序中使用下面引导来代码解决：

```
require('path/to/yii.php');
// disable PHP include path
Yii::$enableIncludePath = false;
Yii::createWebApplication($config)->run();
```  

## “实时”的发送日志信息到目的地
一些控制台命令可能运行了很长一段时间，他们往往需要保持持久性存储一些日志信息来跟踪进度。在Yii的日志记录机制现在可以支持这样的“实时”记录。要做到这一点，长时间运行之前调用代码如下：

```
// automatically send every new message to available log routes
Yii::getLogger()->autoFlush = 1;
// when sending a message to log routes, also notify them to dump the message
// into the corresponding persistent storage (e.g. DB, email)
Yii::getLogger()->autoDump = true;
```
## 使用AR保存记数器

CActiveRecord有一个新的方便的方法叫saveCounters()。它十分像已有的CActiveRecord::updateCounters()。主要区别是saveCounters()只适用于当前的AR对象，而updateCounters()适用于整个数据库表。下面这个例子显示了它们的不同之外：

```
$post = Post::model()->findByPk(1);
// increment the view count of this post by 1
$post->saveCounters(array('views'=>1));
// equivalent implementation
Post::model()->updateCounters(array('views'=>1), 'id=1');
```

## 生成message文件

当使用yiic message命令生成消息文件，你经常要删除旧的message文件，并替换为新生成的呢？您现在有一个选项，强制命令覆盖新生成的旧文件。要做的这一点，在配置文件使用这个命令，添加一个新的选项："overwrite" => true。

## 在控制台应用程序创建URL

我们经常使用CUrlManager创建Web应用程序中通用网址。有时，控制台应用程序也有类似的需求。例如，一个控制台命令可能需要建立一个网站地图的URL的组成为前端Web应用程序。现在就比较容易了，因为我们可以像在Web应用程序中一样调用Yii::app()->createUrl()。

## 剪辑参数和占位符

有时，一遍又一遍重复的代码段，但不值得移动到一个单独的视图文件。在这种情况下，你可以使用剪辑正在接受的参数，将取代剪辑占位符：

```
<?php // defining a clip ?>
<?php $this->beginClip('hello')?>
<p>Hello, {username}!</p>
<?php $this->endClip() ?>
 
<?php // using previously defined clip ?>
<?php $this->renderClip('hello',array(
    '{username}'=>'Qiang',
))?>
<?php $this->renderClip('hello',array(
    '{username}'=>'Alex',
))?>
<?php $this->renderClip('hello',array(
    '{username}'=>'Michael',
))?>
```