## Problem Description

安装zabbix后，浏览器打开输出php源码，而没有执行解析。在`/etc/httpd/conf.d/zabbix.conf`配置文件:

```
<IfModule mod_php5.c>
        php_value max_execution_time 300
        php_value memory_limit 128M
        php_value post_max_size 16M
        php_value upload_max_filesize 2M
        php_value max_input_time 300
        php_value always_populate_raw_post_data -1
        php_value date.timezone Europe/Riga
</IfModule>
```

注释IfModule条件判断，看php模块是否加载:

```
#<IfModule mod_php5.c>
        php_value max_execution_time 300
        php_value memory_limit 128M
        php_value post_max_size 16M
        php_value upload_max_filesize 2M
        php_value max_input_time 300
        php_value always_populate_raw_post_data -1
        php_value date.timezone Europe/Riga
#</IfModule>
```

重启httpd服务失败，错误信息:

```
Invalid command 'php_value', perhaps misspelled or defined by a module not included in the server configuration
```

说明php模块没有加载。修改`/etc/httpd/conf/httpd.conf`文件，加载php模块:

```
LoadModule php5_module "modules/libphp5.so"
```

重启httpd服务发现问题并没有解决，还需要加上以下内容:

```
AddType application/x-httpd-php .php
AddType application/x-httpd-php-source .phps
```
