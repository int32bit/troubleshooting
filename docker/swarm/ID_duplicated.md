#  ID duplicated - Node was already added to the cluster

## Problem Description

Docker Swarm Manage fail to start with following error:

```
ID duplicated - Node was already added to the cluster
```

## Solution

Did you clone the instances by any chance?
That ID is generated the first time the Docker daemon is launched.
The fix is to remove ~/.docker/key.json and let Docker re-generate the ID.

## References

1. [github issue](https://github.com/docker/swarm/issues/362#issuecomment-242256499)
