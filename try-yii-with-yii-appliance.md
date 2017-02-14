A virtual appliance (VMware image) with an installation of Yii 1.0.6 and the following components is now available.

* PHP 5.2.3 (in fast-cgi mode)
* Lighttpd 1.4.7 (mod_rewrite and fast-cgi enable)
* Yii 1.0.6 (including demos)
* PostgreSQL 8.2.4
* phpPgAdmin 4.1.3
* Linux kernel 2.6.17.7

This is a complete pre-built server. That means that the OS and applications already installed and ready for use. Once these servers are opened with VMware Server or Player, you can instantly "turn them on" and use them.

The appliance also contains networking tools such as ssh, dhcp, scp, etc. The appliance contains 2 disk images, the server disk is mounted as read-only while the data disk can grow to 4GB.
How to use these images

1.[Download and install the free VMware Server](http://www.vmware.com/products/server/).
2.[Download Yii 1.0.6 Virtual Appliance (16.9 MB)](http://yii.googlecode.com/files/yii-vmware-1.0.6.zip).
3.Unzip it.
4.Load the image in VMware.
5.To shutdown the server, press the stop button.

The purpose of this appliance is for demonstration purposes only, it should NOT be deployed as a production system.