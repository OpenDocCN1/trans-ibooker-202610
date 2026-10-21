```
spec:
  type: NodePort
  selector:
    app: hello-world
  ports:
  - port: 8080
    targetPort: 8080
    nodePort: 30767
```

In the service manifest, we have mapped Pod port 8080 to NodePort (i.e., physical instance port) 30767. In this way, you can expose the IP directly or place a load balancer of your choice. If you now do a get svc, you can see the mapping of ports as follows:

```
$ kubectl get svc
NAME                 TYPE       CLUSTER-IP     EXTERNAL-IP
PORT(S)              AGE
hello-world-node-service   NodePort   10.32.0.208    <none>
8080:30767/TCP   8s
hello-world-service        ClusterIP  10.32.0.40     <none>
8080/TCP         41m
```

Now we can access the service on the node at port 30767:

```
ubuntu@controller0:~$ curl http://10.240.0.21:30767
Hello World
```

The IP in the curl command is the worker node’s physical IP (not the cluster IP) address, and the port that is being exposed is 30767. You can even directly hit the public IP of the node for port 30767. Figure 4-5 showcases how the cluster IP, node port, and load balancer relate to one another.