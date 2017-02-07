**Yii 关于 jQuery-pjax 的 fork 2.0.6 发布了**

Yii 关于 jQuery-pjax 的 fork 最近发布了一个新的修订版本，改善了性能，并添加了新的配置选项：

* pushRedirect - 是否在 URL 中使用 pushRedirect 作为重定向。默认为 false。
* replaceRedirect - 是否在 URL 中使用 replaceRedirect 作为重定向。默认为 true。
* skipOuterContainers - 当 pjax 的容器是嵌套的而且这个选项设为 true，那么最靠近的 pjax 块会处理这个事件。如果设为 false，最顶层的容器回初理这个事件，默认为 false。默认为 false。
* ieRedirectCompatibility - 是否为 IE 浏览器添加 `X-Ie-Redirect-Compatibility` 的请求头。

如果你的 composer.json version 没有升级到2.0.5版本，运行 composer 更新以便更新库。或者更改版本并且运行 composer 更新。