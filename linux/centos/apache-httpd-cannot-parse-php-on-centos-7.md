## Problem Description

I installed apache httpd and zabbix successfully, but when I open zabbix index page, I see php source without any parse operation.

## Solution

See [apache-httpd-cannot-parse-php-on-centos-7](http://unix.stackexchange.com/questions/181177/apache-httpd-cannot-parse-php-on-centos-7)

Edit `/etc/httpd/conf/httpd.conf` and ensure following content is present:

```
LoadModule php5_module /etc/httpd/modules/libphp5.so
<FilesMatch \.php$>
    SetHandler application/x-httpd-php
</FilesMatch>
```
