and controls the mysql container. The vttablet sidecar initializes the mysql instance using a script contained in the example-cluster-config Secret.

While this configuration doesn’t specifically include details about etcd, the Vitess Operator uses its default settings to create a 3-node etcd cluster to serve as the Topology Service for the VitessCluster. Because of the shortcomings of the StatefulSets, the operator manages each pod and PersistentVolumeClaim individually. This points to the possibility for future improvements as Kubernetes and the operator mature; perhaps the Kubernetes API server can one day serve the role of the Topology Service in the Vitess architecture.

# VISUALIZING LARGER KUBERNETES APPLICATIONS

While its a good exercise to use the kubectl get and kubectl describe commands to explore all of the resources that were created when you installed the operator and created a cluster, you may find it easier to use a tool such as Lens, which offers a friendly graphical interface that allows you to click through the resources more quickly.

At this point, you have a VitessCluster with all of its infrastructure provisioned in Kubernetes. The next steps are to create the database schema and configure your applications to access the cluster using the VTGate Service. You can follow the steps in the blog post Vitess Operator for Kubernetes, which also describes other use cases for managing a Vitess installation on Kubernetes, including schema migration, backup, and restore.

The backup/restore capabilities leverage VitessBackupStorage and VitessBackup CRDs which you may have noticed during installation. VitessBackupStorage resources represent locations where backups can be stored. After you configure the backup section of a VitessCluster and point to a backup location, the operator creates VitessBackup resources as a record of each backup it performs.