# Anthos Multi Cluster Ingress (MCI)

Anthos MCI is a service available under your Anthos subscription. It is built on the architecture of Google's external HTTP(S) Load Balancing service with proxies deployed at over 100 Google **points of presence (PoPs)** around the world. As we see in Figure 8.17, MCI provides a single IP for the app independent of where the app is deployed.

> **Exam Tip**
> MCI should be used when there is a need for load balancing for multi-cluster and multi-regional applications. It's a controller for the external HTTP(S) Load Balancer that provides ingress for traffic coming from the internet to one or more Anthos clusters.

![](bbox=[0.22, 0.365, 0.78, 0.715])

Figure 8.17 – Anthos MCI (Source: https://cloud.google.com/kubernetes-engine/docs/concepts/multi-cluster-ingress)

Anthos MCI comes with two very powerful features supporting the multi cluster setup.