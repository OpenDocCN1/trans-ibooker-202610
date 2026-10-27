
## High availability setup

Availability of the admin cluster is a key requirement for production environments. The default configuration file is used for the new cluster. To check the current configuration and requirements before creating the admin cluster, you can use the `gkectl check-config` command with the `--kubeconfig` flag:

```
gkectl check-config --kubeconfig [ADMIN_CLUSTER_KUBECONFIG] --config
```

This is a function to validate the settings, including the three-node load balancer and the cluster configuration. Then, the load balancer should be created for the user cluster. If you are going to create a new Seesaw cluster before attempting to configure the user cluster, the existing load balancer will not be replaced, and you should back up the cluster configuration. To create the Seesaw load balancer and the kubeconfig, run the following command:

```
gkectl create loadbalancer --kubeconfig [ADMIN_CLUSTER_KUBECONFIG]
```

Configure a new load balancer pair for the external Seesaw service, and then use the following command to create the user cluster:

```
gkectl create cluster --kubeconfig [ADMIN_CLUSTER_KUBECONFIG] --config
```

You can skip this validation by using the `--skip-validation` flag if the cluster configuration has already been validated.

The whole cluster creation process takes about 20–30 minutes, depending on the hardware and the virtual environment, while most of the time is spent waiting for VMware vCenter to create the nodes and for the cluster to join the load balancer.

High availability is a necessary part of Anthos environments. It protects against single points of failure, such as the loss of a single node or a hardware failure, and provides high availability in production environments.

High availability for the admin cluster is implemented by the Anthos High Availability feature in Anthos On-Prem clusters. This ensures that admin clusters are distributed across different physical nodes in an Anthos cluster, so that in the event of a physical node failure, the admin cluster will still be available.

By default, HA is not enabled. To enable HA, specify `usercluster.master.replicas: 3` in the user cluster configuration file. This will create three user cluster masters, which provides high availability for the user cluster control plane.

### 6.5.3 Cluster Management: Scaling

Scaling is performed using the `gkectl` command. You can update the cluster configuration by changing the number of nodes and then execute the update command:

```
gkectl update cluster --kubeconfig [USER_CLUSTER_KUBECONFIG] --config
```

### 6.5.4 Cluster Management: Upgrading Anthos

The upgrade process should be planned carefully. There is no one-time procedure that works for every environment. A step-by-step upgrade process should be followed, including the introduction of the upgrade steps, the upgrade of the admin cluster and the user cluster, and the verification that the cluster is healthy before proceeding.

Before upgrading, you should check the supported upgrade path. From Anthos On-Prem version 1.3.2 onwards, administrators can upgrade directly to a newer version in the same support window. For version 1.7, administrators can upgrade the admin cluster in the same version, while still allowing the admin workstation to be upgraded. During the upgrade, the admin cluster will continue to serve traffic. Administrators should validate the upgrade in a test cluster before applying it to production.

To perform an in-place upgrade, a new node is created with the image of the target version, the workloads on the old nodes are drained, and the old node is then removed. This means that administrators should ensure that there are sufficient resources in the cluster during the upgrade to accommodate the additional temporary nodes. See Figure 6.11.

Figure 6.11. Upgrading flow

![](img/fb3bebce7b5f593bca1a7f8a1371be06_193_0.png)

For more detailed information, see [https://cloud.google.com/anthos/gke/docs/on-prem/how-to/upgrading#upgrading_your_admin_workstation](https://cloud.google.com/anthos/gke/docs/on-prem/how-to/upgrading#upgrading_your_admin_workstation).

### 6.5.5 Cluster Management: Backing up Clusters

Anthos admin clusters can be backed up by following the steps found in [https://cloud.google.com/anthos/clusters/docs/on-prem/1.8/how-to/back-up-and-restore-an-admin-cluster-with-gkectl](https://cloud.google.com/anthos/clusters/docs/on-prem/1.8/how-to/back-up-and-restore-an-admin-cluster-with-gkectl).

It is recommended to use a backup tool that is regularly scheduled and supports the Anthos version you are using. Anthos clusters can be backed up by running a backup script, as described in [https://cloud.google.com/anthos/gke/docs/on-prem/how-to/backing-up](https://cloud.google.com/anthos/gke/docs/on-prem/how-to/backing-up).

When the cluster size is large, a backup can take time. It is important to back up your clusters regularly, and to verify that a backup can be restored in a disaster. For more information, see the Google Cloud blog post: [https://cloud.google.com/blog/products/storage-data-transfer/google-cloud-launches-backups-for-gke](https://cloud.google.com/blog/products/storage-data-transfer/google-cloud-launches-backups-for-gke).

A backup should also include and be able to restore the admin cluster’s configuration and the new cluster from the backup. You should also create a test cluster and restore a backup to verify that the restore process works.

Starting in H2 2021, Google Cloud released a feature named Anthos Cluster Backup for GKE, which allows you to back up a cluster with a single command and store the backup in a cloud storage bucket, and to restore the backup with a few commands.

### 6.6 GKE on AWS

GKE on AWS enables customers to deploy and manage GKE clusters in AWS. It provides a platform for building Kubernetes clusters in AWS, while allowing you to use your existing AWS services. When you create a GKE cluster in the AWS environment, you can use the existing AWS services to manage your cluster. It is important to reiterate ...
