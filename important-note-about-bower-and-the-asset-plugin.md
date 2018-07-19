Bower has changed their registry URL some time ago and has now announced to deprecate the old URL. Old registry will be disabled next Wednesday. In order for your work not to be affected make sure to update your composer asset plugin to the latest version:
```
composer global require "fxp/composer-asset-plugin:^1.4.2"
```
Versions prior to 1.4.2 use the https://bower.herokuapp.com/packages URL for fetching bower packages, which currently redirects to https://registry.bower.io/packages. This URL will be disabled on Wednesday, October 26, 2017 according to the Bower announcement so asset plugin versions prior to 1.4.2 will not be able to fetch any packages.

If you have any questions or comments on this topic, there is a forum topic for discussion.
