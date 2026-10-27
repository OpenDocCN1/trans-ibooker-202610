facto standard protocol interface for reading and writing data into file and block storage.

+   - *Control Plane:* used by Kubernetes volume plugins to allocate, deallocate and manage space in the physical storage.

Unlike the data plane, the control plane interface had not been standardized until very recently. In 2017, an effort across container orchestrators involving Kubernetes, Mesos, Docker, Cloud Foundry and a few others, led to the [Container Storage Interface (CSI)]( ) specification. A storage vendor implements the CSI specification into a driver using its specific APIs and storage protocols. A vendor’s CSI driver is then packaged and deployed in the Kubernetes cluster as Pods, typically separated into a controller deployment and a per-node deployment. Kubernetes calls the appropriate CSI APIs implemented by these drivers when orchestrating volumes for container workloads. CSI will be further explained in the following sessions.

Figure 15.1. Data and Control Planes