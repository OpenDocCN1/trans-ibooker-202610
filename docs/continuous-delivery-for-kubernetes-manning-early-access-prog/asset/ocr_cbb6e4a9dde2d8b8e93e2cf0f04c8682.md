Finally, if you want to expose your Services outside of the Kubernetes Cluster, you need an Ingress resource. As the name represents, this Kubernetes resource is in charge of routing traffic from outside the cluster to services that are inside the cluster. Usually, you will not expose multiple services, limiting the entry points for your applications.
You can get all the available Ingress resources by running the following command:

```
kubectl get ingress
```

The output should look like:

```
NAME       CLASS    HOSTS   ADDRESS     PORTS   AGE
frontend   <none>   *       localhost   80      4m32s
```

Figure 2.X Listing application's Ingress Resources

And then you can describe the Ingress resource in the same way as you did with other resource types to get more information about it:

```
kubectl describe ingress frontend
```

You should expect the output to look like this:

```
Name:                     frontend
Namespace:                default
Address:                  localhost
Default backend:          default-http-backend:80 (<error: endpoints "default-http-backend" not found>)
Rules:
  Host        Path  Backends
  ----        ----  --------
  *
              /     fmtok8s-api-gateway:80 (10.244.1.8:8080)
Annotations:  kubernetes.io/ingress.class: nginx
              meta.helm.sh/release-name: app
              meta.helm.sh/release-namespace: default
              nginx.ingress.kubernetes.io/rewrite-target: /
Events:
  Type    Reason  Age   From                      Message
  ----    ------  ----  ----                      -------
  Normal  Sync    7m58s (x2 over 7m59s)  nginx-ingress-controller  Scheduled for sync
```

Figure 2.X Describing Ingress Resource

As you can see, Ingresses also uses Services name to route traffic. Also, for this to work, you need to have an Ingress Controller, as we installed when we created the KinD Cluster. If you are running in a Cloud Provider you might need to install an Ingress controller.