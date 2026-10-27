might change with new version releases, so it is recommended to refer to the official page for the installation procedure[^6]. Take a look at how we can deploy our example application using Minikube.

Once Minikube is successfully installed, you can start it by running the below command:

```
minikube start
```

Now you can create a simple hello-minikube deployment and expose it using NodePort service. We will use an already existing container image: echoserver:1.4 but you can also build your own image.

+   1. Create a deployment by running:

```
kubectl create deployment hello-minikube --image=k8s.gcr.io/echos
```

+   2. Expose the Deployment by creating a services:

```
kubectl expose deployment hello-minikube --type=NodePort --port=8
```

+   3. Check the service exists:

```
kubectl get services hello-minikube
```

+   4. You should see the following prompt:

NAME            TYPE       CLUSTER-IP       EXTERNAL-IP   PORT(S)
hello-minikube  NodePort   10.102.94.116    <none>        8080:31

+   5. Configure port forwarding for the service to your local machine port:

```
kubectl port-forward service/hello-minikube 8080:8080
```

The following prompt will appear indicating the port is forwarded:

```
Forwarding from 127.0.0.1:8080 -> 8080
Forwarding from [::1]:8080 -> 8080
```

+   6. Now we can open the browser and see that the service is responding on the address http://localhost:8080