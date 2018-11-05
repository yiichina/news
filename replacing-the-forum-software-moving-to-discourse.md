几个月前，我们用 Yii 2 中的重写版本[取代了旧的 Yii Framework 网站](https://www.yiichina.com/news/169)。在开发新网站时，我们还讨论了用更现代的解决方案替换旧的 IPB 论坛软件，但是将论坛与站点一起替换将是太多的工作需要同时完成。

从几年前开始的长时间讨论开始，我们现在已经评估了不同的论坛软件，并决定使用 [Discourse](https://www.discourse.org/)，这是一个由创建 StackOverflow 的人们制作的开源论坛软件。我们将从明天（2018年9月4日）开始用 Discourse 实例取代旧论坛。

以下是将要更改的事项清单：

* 用户帐户将由网站管理，我们现在没有重复登录，所有用户都通过SSO登录论坛。

* 论坛类别，主题和帖子将从旧论坛迁移，因此不会丢失任何内容（我们可能会重新安排类别）。

* 来自旧论坛的链接应全部重定向到新位置。如果您点击破损的链接，请向我们报告！

* 关注的主题和关注的论坛不会被迁移，因此**如果您想获得有关新帖子的通知，请务必访问新论坛并配置您的通知设置**以及更新关注的主题。

* 网站上的[用户徽章](https://www.yiiframework.com/badges)不再包含论坛帖子，而是我们使用 Discourse 的徽章系统，它比以前有更多的徽章。

* Discourse 允许您将其[配置为像邮件列表](https://forum.yiiframework.com/my/preferences/emails)一样，因此如果您希望从您的电子邮件客户端参与 Yii 讨论，您现在可以这样做。

如果您有问题登录到新的论坛，请使用[联系表单](https://www.yiiframework.com/contact)或[聊天](https://www.yiiframework.com/chat)得到帮助。

有一个论坛的[主题讨论](https://www.yiiframework.com/forum/index.php/topic/77008-replacing-the-forum-software-moving-to-discourse/)这个公告。