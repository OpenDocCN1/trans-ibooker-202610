# Network connectivity

Every Anthos GKE cluster has a load balancer. The load balancer either comes bundled during installation or can be configured manually. These load balancers allow Services to be exposed via NodePort to resources running outside the cluster. Each Service gerts a virtual IP address (Service VIP) which is routable and reachable inside the network. Load balancer routes the traffic to the node IP on the Service NodePort which gets forward to the Pod IP on the desired Port.

Services (and Pods) running in one cluster can access the Service VIP for a Service running in another cluster, which gets routed to the desired Pod via the load balancer.

Figure 12.15 Anthos GKE hybrid multi-cluster networking

Anthos GKE Hybrid Multi Cluster Networking

![](bbox=[0.16, 0.55, 0.87, 0.77])

Anthos clusters can also be configured with ingress controllers. Ingress controllers are L7/HTTP load balancers that typically reside inside the cluster. The Ingress controllers themselves are exposed via an L3/L4 load balancer. This way you can use one VIP (the Ingress VIP) for multiple Services running on the same cluster. Ingress controllers act upon Ingress