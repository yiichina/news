我们很自豪地宣布 Yii Framework 1.0.10 版本发布了！

在这次发布中，包含近30个 bug 的修复和小功能的增强。我们修复的一个重要的 bug 是使用 CDbCommand 执行 SQL 语句可能会导致随后的 SQL 执行或者 ActiveRecord 查询失败。我们包含了允许使用透明背景的 CAPTCHA 控件。We added Yii::registerAutoloader() so that third-party class autoloader methods can be more easily hooked in Yii environment. We added support for attaching anonymous functions (PHP 5.3+) as event handlers. We also enhanced CPhpMessageSource so that each extension can manage its own translated messages. Last but not the least, we added CBooleanValidator so that validating boolean values (e.g. checkbox input) is easier. For a list of all enhancements and issues resolved in this release, please see [change logs](http://www.yiiframework.com/files/CHANGELOG-1.0.10.txt).

Upgrading to v1.0.10 from v1.0.9 should be very safe without any known backward compatibility issues. Please check the [upgrade guide](http://www.yiiframework.com/files/UPGRADE-1.0.10.txt) if you are upgrading from 1.0.8 or lower versions.