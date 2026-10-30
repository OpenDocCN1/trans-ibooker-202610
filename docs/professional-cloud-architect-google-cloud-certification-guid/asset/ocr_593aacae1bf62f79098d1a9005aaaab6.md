> **Important Note**
> **YAML Ain't Markup Language (YAML)** is a human-friendly data serialization standard, mainly used for configuration files. It is an alternative for formats such as XML or JSON.

It is essentially a control plane for the cluster. It works as follows:

![](bbox=[0.2, 0.24, 0.78, 0.45])

Figure 6.4 – Control plane

The master node runs multiple processes:

+   - **API server**: Exposes the Kubernetes API. It is the frontend of the control plane.
+   - **Controller manager**: Multiple controllers are responsible for the overall health of the cluster.
+   - **etcd**: A database that hosts the cluster state information.
+   - **Scheduler**: Responsible for placing the Pods across the nodes to balance resource consumption.

A cluster can run perfectly with just one master, but multiple nodes should be run for high availability and redundancy purposes. Without the master, the control plane is essentially down. All the cluster management operations you perform go through the master API. So, for production workloads, it is recommended that you have *multiple master* node configurations.