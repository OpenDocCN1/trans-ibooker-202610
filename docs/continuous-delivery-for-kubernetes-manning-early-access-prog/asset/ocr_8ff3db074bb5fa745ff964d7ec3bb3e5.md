Figure 2.X First replica answering your request

If you now refresh the page, the second replica should answer your request:

Figure 2.X Second replica is now answering; this replica might be on a different node

By default, Kubernetes will load-balance the requests between the replicas. Being able to scale by just changing the number of replicas, there is no need to deploy anything new, Kubernetes will take care of provisioning a new Pod (with a new container in it) to deal with more traffic. Kubernetes will also make sure that there is the amount of desired replicas at all times. You can test this by deleting one pod and watching how Kubernetes recreates it automatically.

# 2.2.2 Connecting services together

We have looked at Deployments, which are in charge of getting our containers up and running and keeping them that way, but so far, these containers can only be accessed inside the Kubernetes Cluster. If we want other services to interact with these containers we need to take a look at another Kubernetes resource called “Service”. Kubernetes provides an advanced Service discovery mechanism that allows services to communicate with each other by just knowing their names. This is essential to connect a large number of services together without the need to use IP addresses of containers that can change over time.

# EXPLORING SERVICES

For exposing your containers to other services, you need to use a `Kubernetes Service` resource. Each application service defines this `Service` resource, so other services and clients can connect to them. In Kubernetes, Services will be in charge of routing traffic to your application containers. These `Service`s represent a logical name that you can use to abstract where your containers are running. If you have multiple replicas of your containers, the Service resource will be in charge of load balance the traffic among all the replicas.

You can list all the services by running:

```
kubectl get services
```

After running the command, you should see something like figure 2.X that follows: