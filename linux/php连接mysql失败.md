## Problem Description

在Centos中php连接mysql失败，页面显示`Can't connect to MySQL server on '192.168.xx.xx' (13)`,检查了mysql数据库的权限没有问题，在本地和远程使用mysql客户端也能正常连接。而后把连接数据库的php脚本抽取出来在本地执行，也连接成功，说明数据库和php脚本都没有问题。

## Solution

通过Google基本确定是SELinux的问题，禁止httpd访问外网，通过`getsebool`命令可以查看SELinux状态:

```
$ getsebool -a  | grep -P '^httpd_can_network_connect\s'
httpd_can_network_connect --> off
```

由输出发现，httpd禁止了网络连接，需要手动开启:

```
setsebool httpd_can_network_connect 1
```
