## Problem Description

In CDH Plugin, if we set `namespace=True` and do'nt allocate floating ip, the progress will hang in `Await starting Cloudera Manager` until timeout.

## Solution

If we use ssh command proxy, the `sahara-engine` should access the vm using network namespace,
but the CDH plugin check cloudera-manager using telnet with manager-ip directly(no namespace, no ssh command proxy) as following:

```python
def _check_cloudera_manager_started(self, manager):
        try:
            conn = telnetlib.Telnet(manager.management_ip, CM_API_PORT)
            conn.close()
            return True
        except IOError:
            return False
```

That because the cloudera manager should be accessed directly without proxies,
see [advanced.configuration.guide](http://docs.openstack.org/developer/sahara/userdoc/advanced.configuration.guide.html#indirect-instance-access-through-proxy-nodes)

In fact, besides checking cloudera-manager service, the `sahara-engine` will call cloudera-manager RESTFul API to deploy Hadoop using manager-ip without proxy too.
So the Cloudera plugin doesn’t support access to Cloudera manager through proxy command.
This means that for CDH clusters must assgine floating ip that the physical node can connect to.

## References

1. [bugs.launchpad.net](https://bugs.launchpad.net/sahara/+bug/1600666)
2. [Sahara Advanced Configuration Guide](http://docs.openstack.org/developer/sahara/userdoc/advanced.configuration.guide.html#indirect-instance-access-through-proxy-nodes)
