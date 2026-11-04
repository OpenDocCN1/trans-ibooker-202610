# Container Optimize Phase

Like the inform phase, the optimize phase in the container world is a direct adaptation of your original FinOps practices built for cloud platforms.

At first glance, it might seem that containerization solves the rightsizing problem. After all, having more things running on the same server instance appears more cost-effective. But as you’ve seen so far, it’s a complex task to measure whether or not you are getting savings from your clusters.

We have heard FinOps practitioners state, “Now that you’ve moved to containers, you don’t need to rightszie.” Or “Idle resources are no longer an issue.” While there are some elements of truth to these statements, they don’t tell the full story. Let’s take a look at how your FinOps practices have to evolve in order to be successful.

# Cluster Placement

If you run all of your containers within one cluster, you may not be able to optimize the cluster for different environments. Running development containers on spot instances, or in a region that is closer to developers, is easier when they run on a development-specific cluster. And further, it helps if you run this development cluster in a development account separate from production.

The alternative is to run multiple clusters, but running too many clusters can be hard to manage and isn’t cost-efficient. Each cluster will have management overhead, both in terms of cloud resources and staff time needs. In addition, it’s harder to maximize cluster utilization when you have workloads spread out over many clusters. The more containers are spread across clusters, the less chance containers will fill your running servers, which means lower utilization.

Finding a balance here is just as important as finding the balance of accounts for the cloud resources that we discussed previously. Working alongside the teams that are managing and using your clusters is the best method of finding the happy medium between smaller domain-specific clusters and larger general clusters within your organization.

# Container Usage Optimization

Now we’ll work through adapting the usage optimizations discussed in a broader optimize phase of the FinOps lifecycle.