the Cloud Monitoring workspace to monitor system components:

+   - **Control plane status**: Showing availability for the clusters core control plane components as API server, scheduler, controller manager, etcd.
+   - **Pod status**: Showing metrics values as CPUusage, memory usage, network traffic, restarts etc...for pods
+   - **Node status**: Showing node metrics as CPU usage, memory usage, allocatable resources, node condition, filesystem size.

# Storage Configuration

The storage: section includes the configuration for the local volume provisioner (LVP) that can be used to provide persistent volumes using mount points on local nodes.

Using local persistent volumes binds the Pod to a specific disk and node. If that disk or node becomes unavailable, then the Pod also becomes unavailable. Due to this, workloads using local PVs need to be resilient to this kind of failure, this is why using local persistent volumes generally fits proof of concepts or advanced use cases where data persistence is not critical or data are replicated to other volumes so are recoverable in case of node/disk unavailability.

There are three types of storage classes for local PVs in an Anthos on bare metal cluster:

+   - LVP node mounts: This storage class creates a local PV for each mounted disk in a specified directory. Each PV maps to a disk with capacity equal to the underlying disk capacity. The total number of local PVs created in the cluster is the number of disks mounted under the path across all nodes.
+   - **LVP share**: This storage class creates a local PV backed by subdirectories in a local, shared file system on every node in the cluster. These subdirectories are automatically created during cluster creation. Workloads using this storage class will share capacity and IOPS because the PVs are backed by the same shared file system.
+   - **Anthos system**: This storage class creates preconfigured local PVs