supports both full and differential backups by creating hard links to the SSTable files it uses for data persistence. Cassandra itself does not take responsibility for copying the SSTable files to backup storage. Instead, this is left to the user. Similarly, recovering from backup involves copying the SSTable files to the Cassandra node where the data is to be reloaded, then Cassandra can be pointed to the local files to restore their contents.

Cassandra’s backup and restore operations are traditionally executed on individual nodes using nodetool, a command line tool that leverages Cassandra’s JMX interface. Cassandra Medusa is an open source command line tool created by Spotify and The Last Pickle that executes nodetool commands to perform backups, including synchronization of backups across multiple nodes. Medusa supports Amazon S3, Amazon S3, Google Cloud Storage (GCS), Azure Storage, and S3 compatible such as MinIO, Ceph Object Gateway, and can be extended to support other storage providers via the Apache Libcloud project.

Medusa can restore either individual nodes to support fast replacement of a downed node, or entire clusters in a disaster recovery scenario. Restoring to a cluster can either be to the original cluster or to a new cluster. Medusa is able to restore data to a cluster with a different size or topology than the original cluster, which has traditionally been a challenge to figure out manually.

K8ssandra has incorporated Medusa in order to provide backup and restore capabilities for Cassandra clusters running in Kubernetes. To configure the use of Medusa in a K8ssandraCluster, you’ll want to configure the medusa properties:

```
apiVersion: k8ssandra.io/v1alpha1
kind: K8ssandraCluster
metadata:
  name: demo
spec:
  cassandra:
    ...
  medusa:
    storageProperties:
      storageProvider: google_storage
```