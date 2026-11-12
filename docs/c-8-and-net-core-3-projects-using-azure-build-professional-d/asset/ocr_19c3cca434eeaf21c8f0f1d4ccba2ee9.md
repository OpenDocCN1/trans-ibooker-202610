From this, we can see that the deployment was far from successful:

```
Events:
  Type     Reason             Age   From                                                              Message
  ----     ------             ---   ----                                                              -------
  Normal   Scheduled          24m   default-scheduler             Successfully assigned default/salesorder...
  ...
  Warning  Failed             23m   (x4 over 24m)   kubelet, aks-nodepool1-40464275-0   Error: ErrImagePull
  Warning  Failed             9m    (x64 over 24m)  kubelet, aks-nodepool1-40464275-0   Error: ImagePullBackOff
  Normal   BackOff            4m    (x86 over 24m)  kubelet, aks-nodepool1-40464275-0   Back-off pulling image "salesorderreg...

```

The issue is that Kubernetes doesn't have permission to pull the image from ACR. We can get a slightly better picture of what's happening by looking at the Kubernetes dashboard. First, we'll need to let the dashboard access our cluster:

```
kubectl create clusterrolebinding kubernetes-dashboard --clusterrole=cluster-admin --serviceaccount=kube-system:kubernetes-dashboard
```

Then, the following command will launch the dashboard:

```
az aks browse --resource-group netcode-projects --name salesorder-cluster
```

> The dashboard is an invaluable tool when you're trying to diagnose issues with Kubernetes!

In fact, in order to pull from ACR, AKS requires explicit permission. We can grant that with the following command:

```
az ad sp create-for-rbac --skip-assignment
```