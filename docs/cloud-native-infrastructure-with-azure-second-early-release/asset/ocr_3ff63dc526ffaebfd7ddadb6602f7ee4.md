```
nginx-deployment    1/1    1    1    105s    nginx
nginx:1.14.2    app=nginx
```

```
$ kubectl get pod
NAME    READY    STATUS    RESTARTS    AGE
nginx-deployment-6b474476c4-bqgh8    1/1    Running    0    4m5s
```

Let’s label the default namespace istio-injection=enabled:

```
$ kubectl label namespace default istio-injection=enabled --overwrite
namespace/default labeled
$ kubectl get namespace -L istio-injection
NAME            STATUS    AGE    ISTIO-INJECTION
default         Active    24m    enabled
istio-system    Active    18m
kube-node-lease Active    25m
kube-public     Active    25m
kube-system     Active    25m
```

Since injection occurs when the Pod is created, we can kill the Nginx Pod and spin a new Nginx Pod, only this time the Pod will have a sidecar injected automatically:

```
$ kubectl delete pod -l app=nginx
pod "nginx-deployment-6b474476c4-bqgh8" deleted

$ kubectl get pod -l app=nginx
NAME    READY    STATUS    RESTARTS    AGE
nginx-deployment-6b474476c4-pfgqv    2/2    Running    0    57s
```

Note that the deployment has spun up two containers. This is the sidecar proxy running in the Pod. You can verify this with describe as follows:

```
$ kubectl describe pod -l app=nginx

Events:
  Type    Reason    Age    From
  Message
```