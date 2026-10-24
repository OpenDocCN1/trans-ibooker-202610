+   ② The label exposed by the Pod to match the Service.
+   ③ The L4 protocol used, TCP or UDP.
+   ④ The port used by this Service.
+   ⑤ The port used by the Pod and mapped into the Service.

To create your Service, run the command as shown below. You also find it in this [book’s GitHub repository](https://github.com/).

```
kubectl create -f service.yaml
```

To check that it is running on Kubernetes:

```
kubectl get svc
```

You should get output similar to:

```
NAME                         TYPE        CLUSTER-IP      EXTERNAL-
IP   PORT(S)   AGE
inventory-quarkus-service    ClusterIP   172.30.34.73    <none>
8080/TCP   6s
```

> **NOTE**
> 
> You just defined a Service, mapped to a Pod. This is only accessible from the internal Kubernetes network, unless you expose it with an object that can accept the traffic from outside the cluster, like Ingress.

# ## Deployment

Deployments are Kubernetes objects created for managing an application lifecycle. A deployment describes a desired state and Kubernetes will implement it using either a *Rolling* or *Recreate* deployment strategy. The rollout lifecycle consists of progressing, complete, and failed states. A