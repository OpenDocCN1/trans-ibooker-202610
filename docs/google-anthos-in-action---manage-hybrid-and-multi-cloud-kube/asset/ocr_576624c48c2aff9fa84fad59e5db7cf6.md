node. To taint the node, we use the kubectl taint command, supplying the node name and the taint.

```
kubectl taint node gke-cls1-pool1-52bcec35-tf0q workload=gpu:NoSc
```

And, label the node to attract the GPU pods.

```
kubectl label node gke-cls1-pool1-52bcec35-tf0q workload=gpu
```

Notice that we did not add a label or a taint to the node in us-east4-c since that zone can run any workload.

Now that the nodes are labeled, you need to create example deployments to verify that workload placement matches the requirements from the table.

# Creating a Deployment that Requires Fast Disk

To force a deployment that requires fast disk access to the **us-east4-a** zone, you need to add a node selector to the deployment. The below manifest creates a NGINX server that contains a nodeSelector using the label disk=fast, forcing the workload to run on a node in the **us-east4-a** zone.

```
apiVersion: v1
kind: Pod
metadata:
    labels:
        run: nginx-fast
    name: nginx-fast
spec:
    containers:
    - image: nginx
      name: nginx-fast
    restartPolicy: Always
    nodeSelector:
        disk: fast
```

When you create and execute the manifest, the nodeSelector tells the scheduler to use a node with the label **disk:fast**. To verify the selector is working correctly, we can list the pods with -o wide to list the node that the pod is running on. In **us-east4-a** we have a single node, gke-cls1-pool1-1d704097-436 - the abbreviated output from our kubectl get pods confirms