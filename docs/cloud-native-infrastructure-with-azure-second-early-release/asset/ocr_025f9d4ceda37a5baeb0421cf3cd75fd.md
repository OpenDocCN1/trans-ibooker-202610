In this chapter, we will dive into the major components and the underlying concepts of Kubernetes. Though this chapter doesn’t aim to teach Kubernetes in full, as there are already a wide array of books¹ that cover the topic at length, we want to build a strong foundation. This hands-on approach will help you better understand the nitty-gritty of the overall environment. Let’s start with a discussion of how the Kubernetes cluster works.

# Kubernetes Components

The Kubernetes cluster contains two types of node components:

*Control plane*

This is the governing component of the Kubernetes cluster. It ensures that a couple of important services (e.g., scheduling, starting up new Pods²) are always running. The core purpose of the control plane node is to make sure the cluster is always in a healthy and correct state.

*Worker nodes*

These are the compute instances that run your workload on your Kubernetes cluster which hosts all your containers.

Figure 4-1 depicts the high-level components of Kubernetes. The lines signify the connections, such as worker nodes accepting a connection talking to the load balancer, which distributes traffic.