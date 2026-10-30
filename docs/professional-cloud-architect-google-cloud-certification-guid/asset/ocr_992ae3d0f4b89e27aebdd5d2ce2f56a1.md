4. To be able to use a Kubernetes ingress resource, we must keep HTTP load balancing enabled:

![](bbox=[0.16, 0.15, 0.86, 0.47])

Figure 6.28 – HTTP load balancing

As you can see, there are a couple of interesting features you can enable in this section, as follows:

+   - **Enable Kubernetes Network Policy**: This allows you to allow or disallow network traffic within the cluster.
+   - **Enable Intranode visibility**: This allows you to gain insight into the network flows between the nodes.
+   - **Enable control plane authorized networks**: This allows you to define which networks you can connect to your control plane from.

Now, we will have a look at the **Security** options.