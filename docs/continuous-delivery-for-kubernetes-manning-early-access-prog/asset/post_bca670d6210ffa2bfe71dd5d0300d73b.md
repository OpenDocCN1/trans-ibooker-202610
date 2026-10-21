
Check the following blog post to see different options of Ingress Controllers that are available for you to use: https://docs.google.com/spreadsheets/u/2/d/191WWNpJj2za6-nbG4ZoUMXMpUK8KICIosvQB0f-og3k/edit#gid=907731238

With Ingresses you can configure a single entry-point and use path-based routing to redirect traffic to each service you need to expose. The previous Ingress resource in Figure 2.20 routes all the traffic sent to `/` to the `fmtok8s-api-gateway` service. Notice that Ingress rules are pretty simple and you shouldn’t add any business logic routing at this level.

## TROUBLESHOOTING INTERNAL SERVICES

Sometimes, it is important to access internal services to debug or troubleshoot services that are not working. For such situations, you can use the `kubectl port-forward` command to temporarily access services that are not exposed outside of the cluster using an Ingress resource. For example, to access the Agenda Service without going through the API Gateway you can use the following command:

```
kubectl port-forward svc/fmtok8s-agenda-rest 8080:80
```

You should see the following output, make sure that you don’t kill the command:

```
Forwarding from 127.0.0.1:8080 -> 8080
Forwarding from [::1]:8080 -> 8080
```

![](img/2ab98a5ff1ec073700d902ba95287fd6_51_0.png)

Figure 2.X `kubectl port-forward` allows you to expose a service for debugging purposes

And then using your browser, curl in a different tab or any other tool to point to http://localhost:8080/info to access the exposed Agenda Service. The following image shows how you can `curl` the Agenda Service info endpoint and print a pretty/colorful JSON payload with the help of `jq`, which you need to install separately.

```
curl localhost:8080/info | jq --color-output
{
  "name": "Agenda Service (REST)",
  "version": "v0.1.0",
  "source": "https://github.com/salaboy/fmtok8s-agenda-rest/releases/tag/v0.1.0",
  "podId": "app-fmtok8s-agenda-rest-7cfc4b5b4c-llqr4",
  "podNamespase": "default",
  "podNodeName": "dev-worker"
}
```

Figure 2.X curl localhost:8080 to access Agenda Service using `port-forward`

In this section, you have inspected the main Kubernetes Resources that were created to run your application’s containers inside Kubernetes. By looking at these resources and their relationships, you will be able to troubleshoot problems when they arise.

For everyday operations, the `kubectl` command line tool might not be optimal and different dashboards can be used to explore and manage your Kubernetes workloads such as: k9s, Kubernetes Dashboard and Octant. You can check Appendix X where the same application is explored using Octant.

## 2.3 Cloud-Native applications Challenges

In contrast to a Monolithic application, which will go down entirely if something goes wrong, Cloud-Native applications shouldn’t crash if a service goes down. Cloud-Native applications are designed for failure and should keep providing valuable functionality in the case of errors. A degraded service while fixing issues is better than having no access to the application at all. In this section, you will change some of the service configurations in Kubernetes to understand how the application will behave in different situations.

In some cases, application/service developers will need to make sure that they build their services to be resilient and some concerns will be solved by Kubernetes or the infrastructure.

This section covers some of the most common challenges associated with Cloud Native applications. I find it useful to know what are the things that are going to go wrong in advance rather than when I am already building and delivering the application. This is not an extensive list, it is just the beginning to make sure that you don’t get stuck with problems that are widely known. The following sections will exemplify and highlight these challenges with the Conference platform.

- **Downtime is not allowed**: If you are building and running a Cloud-Native application on top of Kubernetes and you are still suffering from application downtime, then you are not capitalizing on the advantages of the technology stack that you are using.
- **Service’s built-in resiliency**: downstream services will go down and you need to make sure that your services are prepared for that. Kubernetes helps with dynamic Service Discovery, but that is not enough for your application to be resilient.
- **Dealing with the application state is not trivial**: we have to understand each service infrastructural requirements to efficiently allow Kubernetes to scale up and down our services.
- **Data inconsistent data**: a common problem of working with distributed applications is that data is not stored in a single place and tends to be distributed. The application will need to be ready to deal with cases where different services have different views of the state of the world.
- **Understanding how the application is working (monitoring, tracing and telemetry)**: having a clear understanding on how the application is performing and that it is doing what it is supposed to be doing is essential to quickly find problems when things go wrong.
- **Application Security and Identity Management**: dealing with users and security is always an after-thought. For distributed applications, having these aspects clearly documented and implemented early on will help you to refine the application requirements by defining "who can do what and when".

Let’s start with the first of the challenges.

### 2.3.1 Downtime is not allowed

In section 2.2.x, you increased the number of replicas for the API Gateway; this is a great feature when your services are created based on the assumption that they will be scaled by the platform by creating new copies of the containers running the service. So, what happens when the service is not ready to handle replication, or when there are no replicas available for a given service?

You also scaled the API Gateway to have 2 replicas running all the time. This means that if one of the replicas stop running for any reason, Kubernetes will try to start another one until 2 are running.

![](img/2ab98a5ff1ec073700d902ba95287fd6_53_0.png)

Figure 2.X Two replicas for the API Gateway / User Interface

You can quickly try this self-healing feature of Kubernetes by killing one of the two pods of the API Gateway. You can do this by running the following commands:

```
kubectl get pods
```

Figure 2.X Checking that the two replicas are up and running

Now, copy one of the two Pods Id and delete it:

```
kubectl delete pod <POD_ID>
```

Then list the pods again:

```
kubectl get pods
```

Figure 2.X A new replica is automatically created by Kubernetes as soon as one goes down

You can see how Kubernetes (the ReplicaSet more specifically) immediately creates a new pod when it detects that there is only one running. While this new pod is being created and started, you have a single replica answering your requests until the second one is up and running. This mechanism ensures that there are at least two replicas answering your users’ requests.

![](img/2ab98a5ff1ec073700d902ba95287fd6_55_0.png)

Figure 2.X As soon as Kubernetes detects one pod misbehaving it will kill it and create a new one

If you have a single replica, if you kill the running pod, you will have downtime in your application until the new container is created and ready to serve requests. You can revert back to a single replica with:

```
kubectl scale --replicas=1 deployments/<DEPLOYMENT_ID>
```

Go ahead and try this out, delete only replica available for the API Gateway Pod:

```
kubectl delete pod <POD_ID>
```

![](img/2ab98a5ff1ec073700d902ba95287fd6_56_0.png)

Figure 2.X With a single replica being restarted, there is no backup to answer user requests

Right after killing the pod, try to access the application by refreshing your browser (http://localhost). You should see "503 Service Temporarily Unavailable" in your browser, as the Ingress Controller (not shown in the previous figure for simplicity) cannot find a replica running behind the API Gateway service. If you wait for a bit, you will see the application come back up.
