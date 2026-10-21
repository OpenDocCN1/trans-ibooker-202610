are CPU and memory. Although there are more types of resources that Kubernetes can handle, we will keep it simple here and discuss only CPU and memory.

You can declare two parameters in the Pod manifest: *request* and *limit*. In the request block you tell Kubernetes the minimum resource requirement for your application to operate, and in the limit block you tell Kubernetes the maximum threshold. If your application breaches the threshold in the limit block, it will be terminated or restarted and the Pod will be mostly evicted. For example, in the following Pod manifest, we are placing a max CPU limit of 500m⁴ and a max memory limit of 206Mi,⁵ which means if these values are crossed, the Pod will be evicted and rescheduled on some other node:

```
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
    - image: nginx
      name: nginx
      ports:
        - containerPort: 80
          name: http
  resources:
    requests:
      cpu: "100m"
      memory: "108Mi"
    limits:
      cpu: "500m"
      memory: "206Mi"
```

# Volumes

Some applications require data to be stored permanently, but because Pods are short-lived entities and are frequently restarted or killed on the fly, all data associated with a Pod can be destroyed as well. *Volumes* solve this problem with an abstraction layer of storage disks in Azure. A volume is a way to store, retrieve, and persist data across Pods throughout the