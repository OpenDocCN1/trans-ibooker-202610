

which hold the Pods), the ReplicaSet will reschedule the Pods to a different, healthy node. A ReplicaSet includes the following:

### Selector

ReplicaSets uses Pod labels to find and list the Pods running in the cluster to create replicas in case of a failure.

### Number of replicas to create

This specifies how many Pods should be created.

### Template

This specifies the associated data of new Pods that a ReplicaSet should create to meet the desired number of Pods.

In actual production use cases, you will need a ReplicaSet to maintain a stable set of running Pods by introducing redundancy. But you don’t have to deal with the ReplicaSet directly, since it’s an abstraction layer. Instead, you should use Deployments, which offer a much better way to deploy and manage Pods. A ReplicaSet looks like this:

```
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: myapp-cnf-replicaset
  labels:
    app: cnfbook
spec:
  replicas: 3
  selector:
    matchLabels:
      app: cnfbook
  template:
    metadata:
      labels:
        app: cnfbook
    spec:
      containers:
      - name: nginx
```

```
image: nginx
ports:
  - containerPort: 80
```

To create the ReplicaSet from the preceding configuration, save the manifest as nginx_Replicaset.yaml and apply it. This will create a ReplicaSet and three Pods, shown as follows:

```
$ kubectl apply -f nginx_Replicaset.yaml
replicaset.apps/myapp-cnf-replicaset created
```

```
$ kubectl get rs
NAME                    DESIRED   CURRENT   READY   AGE
myapp-cnf-replicaset    3         3         3       6s
```

```
$ kubectl get pods
NAME                                 READY   STATUS    RESTARTS   AGE
myapp-cnf-replicaset-drwp9           1/1     Running   0          11s
myapp-cnf-replicaset-pgwj8           1/1     Running   0          11s
myapp-cnf-replicaset-wqkl1           1/1     Running   0          11s
```

## Deployments

In production environments, one of the key things that are constant is change. You will keep updating/changing the applications in your production environment at a very rapid pace, since the most common task you will be doing in production is rolling out new features of your application. Kubernetes offers the Deployment object as a standard for doing rolling updates, and provides a seamless experience to the cluster administrator as well as end users. This means you can push updates to your applications without taking down your applications, since Deployment ensures that only a certain number of Pods are down while they are being updated. These are known as zero downtime deployments. By default, deployments ensure that at least 75% of the desired number of Pods are up. Deployments are the reliable, safe, and current way to roll out new versions of applications with zero downtime in Kubernetes.

You can create a deployment as follows:

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx
        ports:
        - containerPort: 80
```

To apply the preceding configuration, save it in a file named nginx_Deployment.yaml and then use kubectl apply as follows:

```
$ kubectl apply -f nginx_Deployment.yaml
deployment.apps/nginx-deployment created
```

Interestingly, the deployment handles ReplicaSets as well, since we declared that we would need three replicas for the deployment Pod. We can check this as follows:

```
$ kubectl get deployment
NAME              READY   UP-TO-DATE   AVAILABLE   AGE
nginx-deployment   3/3     3            3           18s
$ kubectl get rs
NAME                           DESIRED   CURRENT   READY   AGE
nginx-deployment-d46f5678b     3         3         3       29s
$ kubectl get pods
NAME                                    READY   STATUS    RESTARTS   AGE
nginx-deployment-d46f5678b-dpc7p        1/1     Running   0          36s
nginx-deployment-d46f5678b-kdjxv        1/1     Running   0
```

So, from the manifest file, we created three replicas in the .spec.replicas file, and for the Deployment object to find the Pods being managed by nginx-deployment, we make use of the spec.selector field. The Pods are labeled using the template field via metadata.labels.

Now let’s roll out a new version of Nginx. Say we want to pin the version of Nginx to 1.14.2. We just need to edit the deployment manifest by editing the file; that is, changing the version and then saving the manifest file, as follows:

```
$ kubectl edit deployment.v1.apps/nginx-deployment
deployment.apps/nginx-deployment edited
```

This will update the Deployment object, and you can check it as follows:

```
$ kubectl rollout status deployment.v1.apps/nginx-deployment
Waiting for deployment "nginx-deployment" rollout to finish: 1
out of 3 new replicas have been updated...
Waiting for deployment "nginx-deployment" rollout to finish: 1
out of 3 new replicas have been updated...
Waiting for deployment "nginx-deployment" rollout to finish: 2
out of 3 new replicas have been updated...
Waiting for deployment "nginx-deployment" rollout to finish: 2
out of 3 new replicas have been updated...
Waiting for deployment "nginx-deployment" rollout to finish: 2
out of 3 new replicas have been updated...
Waiting for deployment "nginx-deployment" rollout to finish: 2
old replicas are pending termination...
Waiting for deployment "nginx-deployment" rollout to finish: 1
old replicas are pending termination...
Waiting for deployment "nginx-deployment" rollout to finish: 1
old replicas are pending termination...
deployment "nginx-deployment" successfully rolled out
```

The Deployment object ensures that a certain number of Pods are always available and serving while the older Pods are updated. As we already mentioned, by default, no more than 25% of Pods are unavailable while an

update is being performed. Deployment ensures a max surge percentage of 25%, which ensures that only a certain number of Pods are created over the desired number of Pods. So, from the `rollout status`, you can clearly see that at least two Pods are available at all times while rolling out a change. You can get the details for your deployment again by using `kubectl describe deployment`.

> **NOTE**
> 
> We directly issued a deployment command using `kubectl edit`, but the preferred approach is to always update the actual manifest file and then apply `kubectl`. This also helps you keep your deployment manifests in version control. You can use the `--record` or `set` command to update as well.

## Horizontal Pod Autoscaler

Kubernetes supports dynamic scaling through the use of the Horizontal Pod Autoscaler (HPA), where the Pods are scaled horizontally; that is, you can create *n* number of Pods based on observed metrics for your Pod if, for example, you want to increase or decrease the number of Pods dynamically based on the CPU metric being observed. The HPA works via a control loop which, at an interval of 15 seconds by default, checks the resource utilization specified against the metrics (see Figure 4-4).

![](img/8d47d1b5dbca55e784ae602145a894c7_136_0.png)

Figure 4-4. Horizontal Pod Autoscaler at work

It is important to note that to use the HPA, we need metrics-server,⁶ which collects metrics from kubelets and exposes them in the Kubernetes API server through the metrics API for use by the HPA. We first create an autoscaler using kubectl autoscale for our deployment, as follows:

```
$ ~ kubectl autoscale deployment nginx-deployment --cpu-percent=50 --min=3 --max=10
horizontalpodautoscaler.autoscaling/nginx-deployment autoscaled
```

The preceding kubectl command will create an HPA that will ensure that no fewer than three and no more than 10 Pods are used in our nginx-deployment. The HPA will increase or decrease the number of replicas to maintain an average CPU utilization across all Pods of no more than 50%.

You can view your HPA using the following:

```
$ ~ kubectl get hpa
NAME                 REFERENCE                         TARGETS   MINPODS   MAXPODS   REPLICAS   AGE
nginx-deployment     Deployment/nginx-deployment       0%/50%    3         10        3          6m59s
```

We can add the scaling factor on which we can create the autoscaler with the help of an HPA manifest YAML file that is tied to the deployment.

## Service

As we mentioned earlier, the Kubernetes environment is a very dynamic system where Pods are created, destroyed, and moved at a varying pace. This dynamic environment also opens a door to a well-known problem: finding the replica Pods where an application is residing, since multiple Pods are running for a deployment. Pods also need a way to find the other Pods in order to communicate. Kubernetes offers the Service object as an abstraction for a single point of entry to the group of Pods. The Service object has an IP address, a DNS name, and a port that never changes as long as the object exists. Formally known as service discovery, this feature basically helps other Pods/services reach other services in Kubernetes without dealing with the underlying complexity. We will discuss the cloud native service discovery approach in more detail in Chapter 6.

To use a service, you can use the service manifest YAML. Suppose you have a simple “hello world” application already running as a deployment. One default technique to expose this service is to specify the service type as ClusterIP. This service will be exposed on the cluster’s internal IP and will only be reachable from within the cluster, as follows:

