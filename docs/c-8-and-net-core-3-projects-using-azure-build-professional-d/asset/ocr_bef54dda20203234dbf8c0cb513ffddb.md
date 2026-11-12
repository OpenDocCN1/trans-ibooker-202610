This should give you a healthy report of the pods:

```
Type Reason Age From Message
Normal Scheduled 16m default-scheduler Successfully assigned default/salesorderservice-666b8b59f-
wl58x to aks-nodepool1-40464275-1
Normal Pulling 16m kubelet, aks-nodepool1-40464275-1 pulling image "salesorderregistry.azurecr.io/salesorderpro
cess:v2"
Normal Pulled 16m kubelet, aks-nodepool1-40464275-1 Successfully pulled image "salesorderregistry.azurecr.io/s
alesorderservice:v2"
Normal Created 16m kubelet, aks-nodepool1-40464275-1 Created container
Normal Started 16m kubelet, aks-nodepool1-40464275-1 Started container
PS C:\Users\pcmic>
```

Finally, the dashboard should show green:

![](bbox=[0.105, 0.35, 0.9, 0.665])

Now that we have successfully configured our Kubernetes cluster, let's go back to the title of this chapter and investigate the load balancing aspect of Kubernetes.