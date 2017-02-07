**最近曝出的 PHPMailer 和 SwiftMailer 的安全问题**

最近在 [PHPMailer](https://github.com/PHPMailer/PHPMailer)  和 [SwiftMailer](http://swiftmailer.org/) 中宣布了三个安全漏洞：

* 25.12.2016, CVE-2016-10033 [Remote Code Execution vulnerability in PHPMailer](https://legalhackers.com/advisories/PHPMailer-Exploit-Remote-Code-Exec-CVE-2016-10033-Vuln.html)
* 27.12.2016, CVE-2016-10045 [Remote Code Execution vulnerability in PHPMailer](https://legalhackers.com/advisories/PHPMailer-Exploit-Remote-Code-Exec-CVE-2016-10045-Vuln-Patch-Bypass.html)
* 28.12.2016, CVE-2016-10074 [Remote Code Execution vulnerability in SwiftMailer](https://legalhackers.com/advisories/SwiftMailer-Exploit-Remote-Code-Exec-CVE-2016-10074-Vuln.html)

这些漏洞的发现要归功于 Dawid Golunski (https://legalhackers.com) 。

在最初版本中，这三个均在受影响的框架中提到了 Yii，因此我们想对此作出评论来澄清谁受到了影响，以及需要什么行动。

关于 PHPMailer，Yii 从未官方地提供任何和 PHPMailer 有关的邮件组件，也没有在 Yii 团体官方发行的代码中捆绑 PHPMailer。本报告中关于 Yii 的部分均是从 PHPMailer README 拷贝粘贴来的，它声明了你可以与 Yii 结合使用 PHPMailer。在后一个版本的报告中，Yii 再也没被提及过。因为补丁是可用的，如果你使用它，那么所需要的行动仅仅就是升级 PHPMailer 到至少 5.2.20 版本。

SwiftMailer 的情况就截然不同了，我们为它提供了一个扩展：[yii2-swiftmailer](https://github.com/yiisoft/yii2-swiftmailer)。细节如下：

你可以使用 [相关论坛帖子 ]来获取有关方面的反馈。 
(http://www.yiiframework.com/forum/index.php/topic/73470-recent-security-issues-with-phpmailer-and-swiftmailer/).

## 谁受到了影响？

只有使用 Swift_MailTransport 或 PHPMailer 类来发送邮件的用户，这些类使用了 PHP mail() 函数。该函数是默认的传输方法，也 [被 SwiftMailer 扩展默认使用] (https://github.com/yiisoft/yii2-swiftmailer)。不过，只有当攻击者可以设置邮件的 Sender/From 部分时，恶意代码才能被注入。


## 我能做什么来保证我的应用程序安全？

考虑到这些漏洞只跟邮件的 From 部分有关，这也是你应该在你的程序中检查的主要位置。这通常是联系表和留言板脚本中的情况。

当正确地验证了用户的输入时，这也是你无论如何也应该做的，你的程序不会受到任何的影响。

在数据被处理之前，在用户输入的地方直接引入安全是一个很好的做法。那些对用户输入的地址进行了正确地验证的应用，接触到上文所提到的三个漏洞的机会是更小的。

有三个解决方法来保证你应用的安全。

* Yii 的 [EmailValidator](http://www.yiiframework.com/doc-2.0/guide-tutorial-core-validators.html#email)： 据我们所知，容易受这种攻击的邮箱地址都被 email validator 所驳回了。
* PHP's 原生 filter_var() 函数：该函数是专们为验证邮箱地址而设计的。比如： 

```php
public function validateEmail() {
    if (filter_var($this->email, FILTER_VALIDATE_EMAIL) === false) {
        $this->addError($this, 'email' , 'This email contains characters that are not allowed');
    }
}
```

## 这些漏洞的根源是什么？

因为 PHP 函数 [mail()](http://php.net/manual/en/function.mail.php) 没有提供一个单独的参数来指定发送者的邮件地址，只有一个方法来完成它：传递一个字符串给函数的 $additional_parameters 参数，带上 -f 标识，后面跟着邮件地址（比如  -fadmin@example.com）。这会导致 /usr/sbin/sendmail 的执行要带上一串参数传递给 PHP 函数 mail()。比如： `/usr/sbin/sendmail -t -i -fadmin@example.com` 。

在该例中，我们手动地将邮件传递给第三个参数，我们承认它是 CLI 参数，并且它应该被正确地验证和转义。然而，PHPMailer 和 SwiftMailer 都提供了一个便捷的 API，它向开发者隐藏了这个事实。

被发现的漏洞证明了第三个参数没有被可靠地转义，攻击者可以通过一些额外的转义来破解它。这意味着，如果发送者的邮件地址将以一种特别的方式来编写，它将变成被执行的发送邮件命令的一部分。

比如，传递如下的参数来作为第三个参数将会额外增加了参数 -oQ 和 -X，这会进入服务器的文件系统然后造成数据泄露。

`-f"attacker\" -oQ/tmp/ -X/var/www/cache/phpcode.php  some"@email.com`

// 将会造成如下的调用：
`/usr/sbin/sendmail -t -i -f"attacker" -oQ/tmp/ -X/var/www/cache/phpcode.php  some@email.com`

请参考文章开头的安全漏洞报告来学习此次攻击的更多细节。