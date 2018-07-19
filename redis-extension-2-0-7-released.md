我们很高兴的宣布 redis 扩展 2.0.7 版本发布了。

This release makes the extension compatible with PHP 7.2 by refering to BaseObject instead of the Object class in Yii.

It also adds simple ORDER BY support to the ActiveQuery so DataProvider functionaility is now fully supported.

Besides two bug fixes this release also greatly improves stablilty by being able to detect a lost connection to the redis server and Connection can now be configured to reconnect if desired.

See CHANGELOG for details.

Thanks to all community members who helped making this release possible!
