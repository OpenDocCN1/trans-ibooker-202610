which should give you a response similar to this:

```
Kubernetes master is running at https://kubernetes.docker.internal:6443
KubeDNS is running at https://kubernetes.docker.internal:6443/api/v1/
  namespaces/kube-system/services/kube-dns:dns/proxy

To further debug and diagnose cluster problems, use `kubectl cluster-info dump`.
```

The kubectl command is the command line interface to control Kubernetes, and we will be using that to deploy the shopping cart to Kubernetes, both on localhost and on Azure. Furthermore, kubectl can be used to inspect the Kubernetes cluster and to start the Kubernetes dashboard, which gives you a friendly UI for looking inside the Kubernetes cluster.

# 3.4 Creating Kubernetes deployment for the shopping cart

Next, we want to deploy the shopping cart to the Kubernetes cluster we just installed and started. To do that, we need add a manifest file describing the deployment to the shopping cart code base called shopping-cart.yaml. This file contains two major sections:

+   1. A deployment section that specifies which container we want to deploy and how we want it set up:
+      - We want to deploy the shopping cart container.
+      - We want one copy of the container running. We could set this number differently and Kubernetes would take care of running as many instances as we wanted.
+      - The port the container communicates on, port 80 in the case of the shopping cart, just like we saw earlier.
+   2. A service section that configures load balancing in front of the shopping cart. The load balancer makes the shopping cart accessible outside the Kubernetes cluster by giving it an external IP. If we were deploying more than one instance of the shopping cart container, the load balancer would balance the incoming traffic between the shopping cart instances.

These are contents of the shopping-cart.yaml file:

```
kind: Deployment
apiVersion: apps/v1
metadata:
  name: shopping-cart
spec:
  replicas: 1
  selector:
    matchLabels:
      app: shopping-cart
  template:
    metadata:
      labels:
        app: shopping-cart
    spec:
```

Start of the section specifying the container deployment