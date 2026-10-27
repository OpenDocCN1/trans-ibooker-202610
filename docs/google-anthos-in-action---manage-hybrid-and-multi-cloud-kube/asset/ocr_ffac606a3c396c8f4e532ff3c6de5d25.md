+   - Dynamic storage provisioning of volumes and other Kubernetes native storage API functions required by customers;
+   - Use of Kubernetes for deploying a storage CSI driver and its dependencies;
+   - Management of storage for Kubernetes scale up and scale down scenarios; and
+   - Portability of stateful workloads that are using persistent storage.

# 15.3.3 Anthos Backup Services

With well-developed, stable CSI features, users can reliably deploy and run their stateful workloads like relational databases in managed Kubernetes clusters in Anthos. Stateful workloads typically have additional requirements over stateless ones, including the need for backup and storage management.

Anthos provides a simple to use, cloud-native service, namely Backup for GKE, for users to protect, manage and restore their containerized applications and data running in Google Cloud GKE clusters.

Two forms of data are captured in a backup:

+   - *Cluster state backup or config backup:* which consists of a set of Kubernetes resource descriptions extracted from the API server of the cluster undergoing backup; and
+   - *Volume backups:* which consists of a set of GCE Persistent Disk (PD) volume snapshots that correspond to PersistentVolumeClaim resources found in the config backup.

Backup and restore operations in Backup for GKE are selective. When performing a backup, the user may select which workloads to backup, including the option to select “all workloads”. Likewise, when performing a restore, the user may select which workloads to restore, including the option to select “all workloads that were captured in this backup”. You can backup workloads from one cluster and restore them into another cluster.

Restore operations involve a carefully orchestrated re-creation of Kubernetes resources in the target cluster. Once the resources are created, actual restoration of workload functionality is subject to the normal cluster