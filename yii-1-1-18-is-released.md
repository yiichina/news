We are very pleased to announce that Yii Framework version 1.1.18 is released. You can download it at yiiframework.com/download/.

This release is a release of Yii 1.1 that has reached maintenance mode and will, from now on, only receive necessary security fixes. We may also accept fixes to adjust the code for compatibility with PHP 7 if they do not cause breaking changes. This allows you to keep your servers PHP version up to date in the environments where old Yii 1.1 applications are hosted and stay within the version ranges supported by the PHP team. Yii 1.1.18 is compatible with PHP 7.1 that, at the time of this writing, has an announced security support until December 1, 2019.

We recommend to use Yii 2.0 for new projects as well as introducing Yii 2.0 for developing new features in existing Yii 1.1 apps, as described in the Yii 2 guide. Upgrading a whole app to Yii 2.0 will, in most cases, result in a total rewrite so this option provides a way for upgrading step by step and allows you to keep old applications up to date even with low budget.

This release includes a bunch of PHP 7 compatibility fixes and two security improvements. CSRF tokens have been adjusted to use masked token approach to mitigate attacks like the BREACH. Furthermore, the CJavascript::quote() method has been adjusted to properly escape strings passed to it in more complex circumstances and for more encodings than before.

For the complete list of changes in this release, please see the change log.

We would like to express our gratitude to all contributors who have spent their precious time helping improve Yii and made this release possible.

To comment on this news, you can do so in the related forum post.
