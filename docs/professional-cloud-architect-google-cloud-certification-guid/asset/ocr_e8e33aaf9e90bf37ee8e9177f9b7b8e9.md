# Choosing the right load balancer

To choose the right load balancer, let’s have look at the following diagram provided by Google:

![](bbox=[0.1, 0.19, 0.86, 0.52])

1 IPv6 clients are supported for TCP traffic if you configure the load balancer in Premium Tier. IPv6 clients aren’t supported for UDP traffic.
2 Another reason to choose External Network Load Balancing is if you need to ensure that the load balancer is located in a particular region.
3 External network load balancers use regional external IP addresses that are accessible by clients anywhere.
4 Clients in a VPC network or in a network connected to a VPC network.
5 The external HTTP(S) load balancer (classic) can be configured to be regional in Standard Tier.

Figure 10.10 – Load Balancer considerations (Source: https://cloud.google.com/load-balancing/docs/choosing-load-balancer. License: https://creativecommons.org/licenses/by/4.0/legalcode)

Let’s go through the preceding diagram step by step:

+   1. First, we need to choose whether we want the load balancer to be accessible externally or internally. For external exposure, we choose what traffic we want to balance.
+   2. For HTTP and HTTPS traffic, we use an HTTP(S) load balancer.
+   3. For TCP traffic with an SSL offload, we would go for the SSL proxy. If we don’t need the offload SSL but need IPv6 or global scope support, we would choose the TCP proxy.
+   4. Then, we need to check whether we need to preserve client IPs. If yes, we go for the network load balancer; if not, we stay with the TCP proxy. The network load balancer can be also used for UDP traffic that does not need IPv6 or global scope.
+   5. For internal load balancing, use an internal TCP/UDP load balancer.