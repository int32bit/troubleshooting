## Problem Description

`nova-compute` fail to spawn vm, lookup `nova-conductor` and `nova-compute` log:

```
Could not access KVM kernel module: Permission denied 
failed to initialize KVM: Permission denied 
```

## Solution

That because the libvirt has no permission to access `/dev/kvm`, solve this just by changing the `/dev/kvm` owner and restart libvirt daemon service as following:

```
sudo chown root:kvm /dev/kvm
sudo systemctl restart libvirtd
```
