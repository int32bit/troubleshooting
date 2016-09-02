## Problem Description

`use_floating_ips = false` and `use_namespaces = true` in sahara.conf,
and my setup also uses Neutron, so use_neutron = true as well.

When create a cluster , it will dead block in `Wait for instance accessibility` progress until timeout. Lookup sahara log, find following error: 

```
Can't login to node cl1-ngt1-vanilla-hadoop-worker-001 10.0.0.91, reason SSHException: Error reading SSH protocol banner _is_accessible /usr/lib/python2.7/site-packages/sahara/service/engine.py
```

But I successfully run this from the network node (which owns the namespaces and the sahara server) using network namespace:

```
ip netns exec qrouter-26422538-e7ee-428d-b3b8-ed3b57e1e1d6 ssh -i id_rsa.priv cloud-user@10.103.240.2
```

## Solution

It turns out the run_subprocess wasn't spawing any command like it has no right to do so... That's rang a bell :) Check the processes : the sahara server runs as a user called 'sahara'.

I activated the rootwrap config, and now sahara can launch commands on VM:

```bash
if [ ! -f /etc/sudoers.d/sahara ]; then
cat  > /etc/sudoers.d/sahara << EOF
Defaults:sahara !requiretty

sahara ALL = (root) NOPASSWD: /usr/bin/sahara-rootwrap /etc/sahara/rootwrap.conf *
EOF
fi
openstack-config --set /etc/sahara/sahara.conf DEFAULT use_rootwrap True
openstack-config --set /etc/sahara/sahara.conf DEFAULT rootwrap_command "sudo sahara-rootwrap /etc/sahara/rootwrap.conf"
systemctl restart openstack-sahara-engine
```

## References

1. [ask.openstack.org](https://ask.openstack.org/en/question/87430/sahara-cant-login-to-nodes/)
