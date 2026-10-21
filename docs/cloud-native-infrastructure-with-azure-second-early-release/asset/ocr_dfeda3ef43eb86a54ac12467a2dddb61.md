You can modify the `replicas` and `storage` parameters in the cluster definition to match your requirements. We recommend that you run your storage with at least three replicas for redundancy purposes. For example, if you run `kubectl edit ikv.org TikvCluster` and modify `replicas` to 4 and `storage` to 500Gi:

```
apiVersion: tikv.org/v1alpha1
kind: TikvCluster
metadata:
  name: basic
spec:
  version: v4.0.0
  pd:
    baseImage: pingcap/pd
    replicas: 4
    # if storageClassName is not set, the default Storage Class
    of the Kubernetes cluster will be used
    # storageClassName: local-storage
    requests:
      storage: "1Gi"
    config: {}
  tikv:
    baseImage: pingcap/tikv
    replicas: 4
    # if storageClassName is not set, the default Storage Class
    of the Kubernetes cluster will be used
    # storageClassName: local-storage
    requests:
      storage: "500Gi"
    config: {}
```

you will get four replicas of a 500 GB storage partition.

# More on etcd

We’ve utilized etcd (stealthily) throughout this book. Here we will spend some time talking about it in detail.

Etcd is a strongly consistent (Raft-consensus), distributed key-value store that is intended for use in distributed systems. It is designed to gracefully handle leader elections and faults such as network interruptions and