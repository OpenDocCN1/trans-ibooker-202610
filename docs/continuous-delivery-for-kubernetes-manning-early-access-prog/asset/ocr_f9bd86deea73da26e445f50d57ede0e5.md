# ReplicaSets

Having multiple replicas of your containers is an important feature to be able to scale your applications. If your application is experiencing loads of traffic from your users, you can easily scale up the number of replicas of your services to accommodate all the incoming requests. In a similar way, if your application is not experiencing a large number of requests these replicas can be scaled down to save resources. The object created by Kubernetes is called `ReplicaSet`, and it can be queried by running:

```
kubectl get replicaset
```

The output should look like:

| NAME | DESIRED | CURRENT | READY | AGE |
|---|---|---|---|---|
| app-fmtok8s-agenda-rest-7cfc4b5b4c | 1 | 1 | 1 | 3h18m |
| app-fmtok8s-api-gateway-8b4b6d56b | 1 | 1 | 1 | 3h18m |
| app-fmtok8s-c4p-rest-558b8d7bbd | 1 | 1 | 1 | 3h18m |
| app-fmtok8s-email-rest-7795df6885 | 1 | 1 | 1 | 3h18m |

Figure 2.X Listing Deployment's Replica Sets

These `ReplicaSet` objects are fully managed by the Deployment` resource, and usually, you shouldn't need to deal with them.

ReplicaSets are also essential when dealing with Rolling Updates, and you can find more information about this topic here: https://kubernetes.io/docs/tutorials/kubernetes-basics/update/update-intro/. You will be performing updates to the application with Helm in later chapters where these mechanisms will kick in.

If you want to change the number of replicas for a deployment you can use once again `kubectl` to do so:

```
kubectl scale --replicas=2 deployments/<DEPLOYMENT_ID>
```

You can try this out with the API Gateway deployment:

```
kubectl scale --replicas=2 deployments/app-fmtok8s-api-gateway
```

This command changes the deployment resource in Kubernetes and triggers the creation of a second replica for the API Gateway deployment. Increasing the number of replicas of your user-facing services is quite common as it is the service that all users will hit when visiting the conference page.

If you access the application again in your browser, at the top of the screen, right beside the API Gateway Service version number, you can see the Pod name and the Node where the Pod is running: