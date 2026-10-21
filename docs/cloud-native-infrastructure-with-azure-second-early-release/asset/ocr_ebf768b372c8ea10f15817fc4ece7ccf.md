stores are great for storing images, videos, and other files that aren’t suitable for a relational database. Blob storage comes with extra challenges around storing and replicating large amounts of data efficiently.

Rook is a cloud native file, block, and object open source storage orchestrator that provides software-defined blob storage. Rook graduated as a CNCF project in 2018 and is specifically designed around running storage on Kubernetes. Similar to Vitess, Rook manages a lot of the daily operations of a cluster, including self-scaling and self-healing, and it automates other tasks such as disaster recovery, monitoring, and upgrades.

# The Rook Architecture

Rook is a Kubernetes orchestrator, not an actual storage solution. Rook supports the following storage systems:

+   - Ceph, a highly scalable distributed storage solution for block storage, object storage, and shared filesystems with years of production deployments
+   - Cassandra, a highly available NoSQL database featuring lightning-fast performance, tunable consistency, and massive scalability
+   - NFS, which allows remote hosts to mount filesystems over a network and interact with those filesystems as though they are mounted locally

Rook can orchestrate a number of storage providers on a Kubernetes cluster. Each has its own operator for deploying and managing the resources.

# Deploying Rook on Kubernetes

In this example, we will deploy a Cassandra cluster using the Rook (Cassandra) Kubernetes operator. If you’re deploying a Ceph cluster via Rook, you can also use a Helm chart to deploy.