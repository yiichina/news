我们很高兴的宣布 Queue 扩展 2.0.1 版本发布了。此版本修复了一些 bugs 并且添加了一些改进，下面我们回顾一下。

添加控制台命令来删除作业和清除整个队列。两者都依赖于队列代理驱动程序，因此可以在特定驱动程序的更新指南中找到详细信息。
添加了支持 Igbinary 作业的序列。
允许改变 RabbitMQ 的 vhost 设置。
现在此扩展兼容 PHP 7.2。
一些类和接口被标记为已过时。参阅 UPGRADE 查看详细信息。
A benchmark measuring time to get the job to the worker was performed As expected, fastest implementations are Gearman, Beanstalk, Radis and RabbitMQ. Average time is 1-2 ms. Details could be found in this gist.
