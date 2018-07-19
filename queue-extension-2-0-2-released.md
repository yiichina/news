我们已经发布了 Queue 扩展 2.0.2 版本。此版本修复了一些 bug 以及添加一些增强功能。

新的AMQP互操作驱动程序添加。 它允许你无缝地使用许多驱动程序队列互操作项目支持。

添加了新事件：cli\Queue::EVENT_WORKER_START 和 cli\Queue::EVENT_WORKER_STOP。这些使你能够添加操作者到 worker 的开始和结尾。

重新定义的退出代码对连接 workers 监测或系统是有用的。

为了操作 posix-signals 添加了 LoopInterface 和 它的 SignalLoop 接口。 Out of the box SignalLoop provides an ability to exit the loop using a signal. 此外，它支持查询延迟。也可很好的配置退出，暂停和恢复信号。

CLI 驱动除了AMQP 和 AMQP Interop 都支持新事件和 LoopInterface。
