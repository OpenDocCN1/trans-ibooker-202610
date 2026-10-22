```
tier: frontend
type: LoadBalancer
```

Note that the service is of type LoadBalancer, which should make it fairly simple to access from your local machine. Execute the command `kubectl get services` to get the load balancer’s IP address, then you can open the Wordpress instance in your browser with the URL *http://<ip>*. Try logging in and creating some pages.

# ACCESSING SERVICES FROM KUBERNETES DISTRIBUTIONS

The exact details of accessing services will depending on the Kubernetes distribution you are using and whether you’re deploying apps in production, or just testing something quickly like we’re doing here. If you’re using a desktop Kubernetes distributions, you may wish to use a NodePort service instead of LoadBalancer for simplicity. You can also consult the documentation for specific instructions on accessing services, such as those provided for Minikube or K3d.

When you’re done experimenting with your Wordpress instance, you can clean up the resources specified in the configuration files you’ve used in the local directory using the command, including the data stored in your PersistentVolumeClaim:

```
kubectl delete -k ./
```

At this point, you might be feeling like this was relatively easy, despite our claims of doing things “the hard way”. And in a sense, you’d be right. So far, we’ve deployed a single node of a simple database with sane defaults that we didn’t have to spend much time configuring. Creating a single node is of course fine if your application is only going to store a small amount of data. Is that all there is to deploying databases on Kubernetes? Of course not! Now that we’ve introduced a few of the basic Kubernetes resources via this simple database deployment, it’s time to step up the complexity a bit. Let’s get down to business!