# 11.3.3 Knative Architecture

Let’s have a look at Knative architecture presented in the below diagram. As you can already notice there are multiple layers with some of the components being plug and play or optional.

# Figure 11.3. Knative Architecture

![](bbox=[0.127, 0.266, 0.884, 0.716])

Knative can run on any compute platform that has the ability to run Kubernetes. It can be either based on virtual machines or bare-metal servers. For traffic routing, Service Mesh Gateway is used. Obviously the most popular is Istio but alternative solutions are also supported like: Gloo, Ambassador, Contour and Kourier with more to come. To learn more about Istio refer to Chapter 5 Anthos Service Mesh: security and observability at scale. On top of that we have Knative components installed as a Kubernetes application. Note that each of those components can be installed and operated