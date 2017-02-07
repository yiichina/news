**Debug toolbar 扩展 2.0.7 版本发布了**

我们非常高兴地宣布 [Debug Toolbar](https://github.com/yiisoft/yii2-debug) 扩展 2.0.7 版本发布了。

在这个版本中有超过二十个bug修复，增强和三个新功能。 修复和增强的目的主要是在边缘情况下获得更好的稳定性，并提高可用性。 这些功能需要单独的描述。

## 时间轴面板

新的时间轴面板在时间线上以图形方式显示分析数据，从初始化开始，到应用程序完成发送响应时结束。它允许您快速查看应用程序的执行顺序，并找出应用程序的工作原理和显示哪些区域有改善性能的机会。

![bJLqoL1.png](/uploads/images/201611/25093635621_thumb.png "bJLqoL1.png")

## AJAX请求处理

AJAX 请求现在由调试面板自动拾取，它们的计数显示在 logo 后面。 您可以通过计数器面板从列表中访问每个单独的请求数据。

![EHtvC1d.png](/uploads/images/201611/25093713422_thumb.png "EHtvC1d.png")

## IDE链接

为什么不能从调试跟踪中直接打开文件？嗯，你现在可以了。 通过几个设置，您可以点击backtrace中的任何文件。 您最喜欢的IDE将自动打开右边的文件。 这将会节省很多时间！

非常感谢：

* [Dmitriy Bashkarev](https://github.com/bashkarev) 用于处理时间轴面板和AJAX请求处理。
* [Thiago Talma](https://github.com/thiagotalma) 他将IDE集成到我们的调试工具栏中做出的努力。

有关更新的完整列表，请参阅 [CHANGELOG](https://github.com/yiisoft/yii2-debug/blob/2.0.7/CHANGELOG.md) 。