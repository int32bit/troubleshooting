## Problem Description

Fail to create CDH cluster with error on `Fnish cluster starting` progress, I ssh into hadoop namenode vm and find no configration in `/etc/hadoop`:

```
int32bit-hadoop-1-cdh-master-00 hdfs]# hdfs dfs -df 
Filesystem Size Used Available Use% 
file:/// 20061442048 5187198976 14874243072 26% 
```

```
int32bit-hadoop-1-cdh-master-00 hdfs]# cat /etc/hadoop/conf/core-site.xml 
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?> 

<configuration> 
</configuration> 
```

Here's my Master Node Template Processes: 

```
CLOUDERA_MANAGER 
YARN_JOBHISTORY 
YARN_RESOURCEMANAGER 
HDFS_SECONDARYNAMENODE 
HDFS_NAMENODE 
```

And Slave processes: 

```
HBASE_REGIONSERVER 
HDFS_DATANODE 
YARN_NODEMANAGER 
```

## Solution

Seems like an issue around Cloudera(not sure). I faced with something like that when cluster was deploy without Oozie.
Deploy cluster with Oozie server may solve this problem.

## References

1. [Mailing List Archive](http://www.gossamer-threads.com/lists/openstack/dev/56854?page=last)
