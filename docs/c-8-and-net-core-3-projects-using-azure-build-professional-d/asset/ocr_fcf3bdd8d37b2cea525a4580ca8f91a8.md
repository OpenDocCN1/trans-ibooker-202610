# Load balancing

The title of this chapter is A Load-Balanced Order Processing Microservice Using Docker and Azure Kubernetes Service. In fact, you've already seen this since the worker pods were picking up items from the queue based on which were available. However, let's see if we can engineer a more dramatic demonstration of what that actually means. Let's go into the dashboard and delete one of the pods; select the ellipsis to the right-hand side of a pod and delete it:

![](bbox=[0.105, 0.28, 0.895, 0.378])

Once you've deleted the pod, you should (almost instantly) notice the screen change to look something like this:

![](bbox=[0.105, 0.45, 0.895, 0.75])