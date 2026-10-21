+   - Debugging commands for getting information about workloads
+   - Debugging commands for manipulating and interacting with Pods
+   - General cluster management commands

In this section, we will explore Kubernetes in depth by focusing on basic cluster commands for managing Kubernetes clusters, Pods, and other objects with the help of kubectl.

# General Cluster Information and Commands

The first step to interacting with the Kubernetes cluster is to learn how to gain insights on the cluster, infrastructure, and Kubernetes components that you’ll be working with. To see the worker nodes that run the workloads in your cluster, issue the following kubectl command:

```
$ ~ kubectl get nodes
NAME      STATUS   ROLES     AGE   VERSION
worker0   Ready    <none>    11d   v1.17.3
worker1   Ready    <none>    11d   v1.17.3
worker2   Ready    <none>    11d   v1.17.3
```

This will list your node resources and their status, along with version information. You can gain a bit more information by using the -o wide flag in the get nodes command as follows:

```
$ ~ kubectl get nodes -o wide
NAME      STATUS   ROLES     AGE   VERSION   INTERNAL-IP   EXTERNAL-IP   OS-IMAGE          KERNEL-VERSION     CONTAINER-RUNTIME
worker0   Ready    <none>    11d   v1.17.3   10.240.0.20   <none>        Ubuntu 16.04.7 LTS   4.15.0-1092-azure   containerd://1.3.2
worker1   Ready    <none>    11d   v1.17.3   10.240.0.21   <none>        Ubuntu 16.04.7 LTS   4.15.0-1092-azure   containerd://1.3.2
worker2   Ready    <none>    11d   v1.17.3   10.240.0.22   <none>        Ubuntu 16.04.7 LTS   4.15.0-1092-azure   containerd://1.3.2
```

To get even more details specific to a resource or worker, you can use the describe command as follows: