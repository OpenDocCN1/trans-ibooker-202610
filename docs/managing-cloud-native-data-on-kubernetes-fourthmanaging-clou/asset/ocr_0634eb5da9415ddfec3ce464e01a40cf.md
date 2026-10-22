Now examine the list of CRDs that have been created as follows:

```
% kubectl get crd
NAME                                                       CREATED AT
cassandrabackups.medusa.k8ssandra.io                       2022-02-05T17:31:35Z
cassandradatacenters.cassandra.datastax.com                2022-02-05T17:31:35Z
cassandrarestores.medusa.k8ssandra.io                      2022-02-05T17:31:35Z
cassandratasks.control.k8ssandra.io                        2022-02-05T17:31:36Z
certificaterequests.cert-manager.io                        2022-02-05T17:31:16Z
certificates.cert-manager.io                               2022-02-05T17:31:16Z
challenges.acme.cert-manager.io                            2022-02-05T17:31:16Z
clientconfigs.config.k8ssandra.io                          2022-02-05T17:31:36Z
clusterissuers.cert-manager.io                             2022-02-05T17:31:17Z
issuers.cert-manager.io                                    2022-02-05T17:31:17Z
k8ssandraclusters.k8ssandra.io                             2022-02-05T17:31:36Z
orders.acme.cert-manager.io                                2022-02-05T17:31:17Z
reapers.reaper.k8ssandra.io                                2022-02-05T17:31:36Z
replicatedsecrets.replication.k8ssandra.io                 2022-02-05T17:31:36Z
stargates.stargate.k8ssandra.io                            2022-02-05T17:31:36Z
```

As you can see, there are several CRDs associated with the Cert Manager and K8ssandra. There is also the cassandradatacenter CRD used by Cass Operator. The K8ssandra and Cass Operator CRDs are all namespaced, which you can verify using the kubectl api-resources command, meaning that resources created according to these definitions are assigned to a specific namespace. That command will also show you the acceptable abbreviations for each resource type, for example k8c for k8ssandracluster.