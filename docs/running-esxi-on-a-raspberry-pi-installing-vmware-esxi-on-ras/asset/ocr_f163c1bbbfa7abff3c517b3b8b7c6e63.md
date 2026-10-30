# Kubernetes

Kubernetes (K8s) is gaining traction at an astounding speed in terms of enterprise adoption. K8s is an orchestration system for containers, and its orchestration functions include the deployment and scaling of container-based applications. It is open source software that was originally developed by Google, but eventually transferred to the Cloud Native Computing Foundation. There are many nuances regarding the differences between a VM and containers that are beyond the scope of this book, but (with apologies to those who know better) they can be thought of as very lightweight VMs.

All the major Linux distributions support running some form of K8s or containers. By using ESXi with VMs, you can have multiple instances of the K8s and container engine running on the same physical server for redundancy or to mimic a larger environment (Figure 3-8).

![](bbox=[0.09, 0.49, 0.88, 0.83])

Figure 3-8. Kubernetes