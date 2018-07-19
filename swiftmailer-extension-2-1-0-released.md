We are very pleased to announce the release of the SwiftMailer extension version 2.1.0, which uses the latest SwitMailer versions - 6.x.

The high level interface of the extension has not been changed, thus any existing code based on 2.0.x version will likely continue to function with 2.1.x as well. However, there are some backward compatibility breaks introduced.

In particular: minimum required version of PHP has been raised to 7.0 and default transport was changed to Swift_SendmailTransport as Swift_MailTransport is no longer available. Check the UPGRADE instructions for migration from 2.0.x versions.

Also make sure your composer.json has the necessary version constraint limit if you want to stay with the 2.0.x version using SwiftMailer 5.x, e.g. ~2.0.7. Check the Composer documentation on version constraints to learn how that works.
