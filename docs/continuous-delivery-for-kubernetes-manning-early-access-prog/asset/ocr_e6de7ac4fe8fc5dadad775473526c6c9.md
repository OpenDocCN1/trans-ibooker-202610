# 2.3.1 Downtime is not allowed

In section 2.2.x, you increased the number of replicas for the API Gateway; this is a great feature when your services are created based on the assumption that they will be scaled by the platform by creating new copies of the containers running the service. So, what happens when the service is not ready to handle replication, or when there are no replicas available for a given service?

You also scaled the API Gateway to have 2 replicas running all the time. This means that if one of the replicas stop running for any reason, Kubernetes will try to start another one until 2 are running.

![](bbox=[0.205, 0.325, 0.81, 0.595])

Figure 2.X Two replicas for the API Gateway / User Interface

You can quickly try this self-healing feature of Kubernetes by killing one of the two pods of the API Gateway. You can do this by running the following commands:

```
kubectl get pods
```