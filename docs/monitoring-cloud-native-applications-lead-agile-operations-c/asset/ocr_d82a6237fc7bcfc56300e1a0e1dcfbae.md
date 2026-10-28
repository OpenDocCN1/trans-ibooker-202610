There are pros and cons with both options. In the first option of stand-alone deployment, you have to take care of the networking between the nodes and configure Prometheus to collect the metrics from the other nodes that are present. This can be done by configuring a container networking platform like Flannel or Calico. In the second option of combined deployment, if you need to upgrade your pods, you also need to ensure that associated history is maintained and backed up, so that Prometheus doesn’t lose the historical data during a rolling upgrade. Therefore, it is easier to run it alongside your workloads rather than as a separate setup if there are no other constraints. For the purposes of this book, we will follow the second option.

The installation steps mentioned in the following assumes that a Kubernetes cluster has already been set up. Readers are advised to use Minikube or Kind to setup a Kubernetes cluster by following instructions available at https://kubernetes.io/docs/tasks/tools/.

Once Minikube is installed, start the Kubernetes cluster (as shown in Figure 4-2) using the following command:

```
$ minikube start
```

![](bbox=[0.095, 0.475, 0.93, 0.645])

Figure 4-2. Starting the single-node Kubernetes cluster

Once the Kubernetes cluster is up and running and kubectl has been installed successfully, the first thing to do is to list all the pods in the cluster (see Figure 4-3) using the following command:

```
$ kubectl get pods -all-namespaces
```