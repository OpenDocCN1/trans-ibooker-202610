
kind: Cluster
metadata:
  name: rook-cassandra
  namespace: rook-cassandra
spec:
  version: 3.11.6
  repository: my-private-repo.io/cassandra
  mode: cassandra
  annotations:
  datacenter:
    name: us-east2
    racks:
      - name: us-east2
        members: 3 # Change this number up or down

```
$ kubectl -n rook-cassandra describe clusters.cassandra.rook.io rook-cassandra
```

> **NOTE**
> 
> You can find more details on Cassandra configurables in the Cassandra CRD documentation.

Similarly, Ceph and NFS clusters can be easily deployed using the Rook operator.

## Introduction to TiKV

TiKV (Titanium Key-Value), created by PingCAP, Inc., is an open source, distributed, and transactional key-value store. Contrary to many key-value and NoSQL systems, TiKV provides simple (raw) APIs as well as transactional APIs that provide atomicity, consistency, isolation, and durability (ACID) compliance.

## Why Use TiKV?

TiKV provides the following features:

- Geo-replication
- Horizontal scalability
- Consistent distributed transactions
- Coprocessor support
- Automatic sharding

TiKV is an attractive option due its auto-sharding and geo-replication features, as well as its ability to scale to store more than 100 TB of data. It also provides strong consensus guarantees with its use of the Raft protocol for replication.

## The TiKV Architecture

As previously mentioned, TiKV has two APIs that can be used for different use cases: raw and transactional. Table 9-1 outlines the differences between these two APIs.

## Table 9-1 TiKV APIs: Raw versus Transactional

|  | Raw | Transactional |
|---|---|---|
| Description | A lower-level key-value API for interacting directly with individual key-value pairs | A higher-level key-value API that provides ACID semantics |
| Atomicity | Single key | Multiple keys |
| Use when... | Your application doesn’t require distributed transactions or multiversion concurrency control (MVCC) | Your application requires distributed transactions and/or MVCC |

TiKV utilizes RocksDB as a storage container in each node, and utilizes Raft groups to provide distributed transactions, as shown in Figure 9-2. The placement driver acts as a cluster manager and ensures that all shards have had their replication constraints met and that the data has been load-balanced over the pool. Finally, clients connect to TiKV nodes using the Google Remote Procedure Call (gRPC) protocol, which is optimized for performance.

![](img/8d47d1b5dbca55e784ae602145a894c7_324_0.png)

Figure 9-2. The TiKV architecture

Within a TiKV node (pictured in Figure 9-3), RocksDB provides the underlying storage mechanisms and Raft provides consensus for transactions. The TiKV API provides an interface for clients to interact with, and the coprocessor handles SQL-like queries and assembles the result from the underlying storage.

![](img/8d47d1b5dbca55e784ae602145a894c7_324_1.png)

Figure 9-3. TiKV instance architecture

## Deploying TiKV on Kubernetes

TiKV can be deployed via multiple automation methods, including Ansible, Docker, and Kubernetes. In the following example, we will deploy a basic cluster with Kubernetes and Helm.

- Install the TiKV custom resource definition:
  
  ```
  $ kubectl apply -f
  https://raw.githubusercontent.com/tikv/tikv-operator/master/manifests/crd.v1beta1.yaml
  ```
- We will need to explore the Helm operator. First, add the PingCap repository:
  
  ```
  $ helm repo add pingcap https://charts.pingcap.org/
  ```
- Now create a namespace for the tikv-operator:
  
  ```
  $ kubectl create ns tikv-operator
  ```
- Install the operator:
  
  ```
  $ helm install --namespace tikv-operator tikv-operator
  pingcap/tikv-operator --version v0.1.0
  ```
- Deploy the cluster:
  
  ```
  $ kubectl apply -f
  https://raw.githubusercontent.com/tikv/tikv-operator/master/examples/basic/tikv-cluster.yaml
  ```
- You can check the status of the deployment by running:
  
  ```
  $ kubectl wait --for=condition=Ready --timeout 10m
  tikvcluster/basic
  ```
  
  This will create a single-host storage cluster with 1 GB of storage and a placement driver (PD) instance.

You can modify the `replicas` and `storage` parameters in the cluster definition to match your requirements. We recommend that you run your storage with at least three replicas for redundancy purposes. For example, if you run `kubectl edit tikv.org TikvCluster` and modify `replicas` to 4 and `storage` to 500Gi:

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

## More on etcd

We’ve utilized etcd (stealthily) throughout this book. Here we will spend some time talking about it in detail.

Etcd is a strongly consistent (Raft-consensus), distributed key-value store that is intended for use in distributed systems. It is designed to gracefully handle leader elections and faults such as network interruptions and

unexpected node downtime. Etcd has a simple API (Figure 9-4), so you will find a number of the systems we’ve discussed in this book (e.g., Kubernetes, TiKV, Calico, and Istio) leveraging it behind the scenes.

Often, etcd is used to store configuration values, feature flags, and service discovery information in a key-value format. Etcd allows clients to watch these items for changes and reconfigures itself when they change. Etcd also is suitable for leadership elections and locking uses. Etcd is used as the service discovery backend in Kubernetes as well as for orchestration in Rook.

![](img/8d47d1b5dbca55e784ae602145a894c7_327_0.png)

Figure 9-4. Etcd operational architecture

If you are utilizing etcd for your core infrastructure, you will quickly understand how important it is for this infrastructure to run reliably. As an etcd cluster is a critical piece of your infrastructure, we recommend the following to ensure a stable etcd cluster in Azure.

## Hardware Platform

Consistent key-value store performance relies heavily on the performance of the underlying hardware. A poorly performing consistent key-value cluster can severely impact the performance of your distributed system. The same applies for etcd. If you are doing hundreds of transactions per second, you should utilize a managed solidstate drive (SSD) with your virtual machine (we recommend a premium or ultra SSD) to prevent performance

degradation. You will need at least four cores for etcd to run, and the throughput will scale with an increase in CPU cores. You can find more information on capacity planning and hardware configurations for the cloud on the general hardware page and the AWS guide (etcd doesn’t currently publish an Azure-specific guide).

We recommend running etcd on the following Azure SKUs:

- Standard_D8ds_v4 (eight-core CPU)
- Standard_D16ds_v4 (16-core CPU)

These SKUs explicitly allow you to attach premium or ultra SSDs, which will increase the performance of the cluster.

> **NOTE**
> As with any production use case that we’ve discussed in this book, we strongly recommend that you enable accelerated networking on all Azure compute instances.

## Autoscaling and Auto-remediation

We would be remiss if we did not discuss how autoscaling and auto-remediation work with etcd. Autoscaling is not recommended for etcd as it could have unintended consequences if capacity additions or subtractions adversely affect cluster performance. That said, enabling auto-remediation (auto-healing) of bad etcd instances is considered to be OK. You can disable autoscaling for etcd by running the following command on your Kubernetes cluster:

```
$ kubectl delete hpa etcd
```

This will disable the Horizontal Pod Autoscaler (HPA), which is used in Kubernetes to automatically scale clusters up and down. You can find more information about the HPA in the Kubernetes documentation.
