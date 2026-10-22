![](bbox=[0.15, 0.08, 0.82, 0.38])

Figure 6-1. K8ssandra Architecture

In the following sections, we’ll take a look at each component of the K8ssandra project to understand the role that it plays within the architecture and its relationship to other components.

# Installing the K8ssandra Operator

Let’s dive in with some hands-on experience of installing K8ssandra. To get a basic installation of K8ssandra running that fully demonstrates the power of the operator, you’ll need a Kubernetes cluster with several worker nodes. To make the deployment simpler, the K8ssandra team has provided scripts to automate the process of creating Kubernetes clusters and then deploying the operator to these clusters. These scripts use Kind clusters for simplicity, so you’ll want to make sure you have this installed before starting. Instructions are also available for installing on various clouds on the K8ssandra website.