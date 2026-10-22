make it work. Organizations need reliable and fast ways to deploy applications, including analytics, and because you are reading this book, you already know the answer. We need to do more with less.

The laziness Dr. Chirica was talking about is a tongue-in-cheek way of describing an ideal future. Instead of having a hundred-person team working night and day to analyze a petabyte of data, what if you could reduce that to one person and a few minutes? The cloud native way of running data infrastructure is a path we should all work towards the goal of that kind of glorious laziness.

We’ve already looked at several aspects of moving stateful workloads onto Kubernetes, including storage, databases, and streaming, and now it’s time to look at analytics to complete the picture. As a bit of a preview, Figure 9-1 shows how Data Analytics fits as the final part of our roadmap of managing the complete data stack using Kubernetes. There are no more external network requirements bridging to resources in or out of the Kubernetes cluster, just a single, virtual data center that serves our bespoke needs for cloud native applications.

![](bbox=[0.26, 0.53, 0.77, 0.84])

Figure 8-1. The Cloud Native Virtual Datacenter