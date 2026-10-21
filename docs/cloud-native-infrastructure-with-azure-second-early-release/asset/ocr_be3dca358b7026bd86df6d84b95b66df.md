Containerd is an OCI-compliant container runtime that is used in Docker. Containerd acts as a layer of abstraction between all the Linux syscalls that make a container operate and the standard OCI configuration that configures a container. Containerd has an events subsystem that allows other systems like etcd to subscribe to system changes and act accordingly. Notably, containerd supports both OCI and Docker image formats. What is containerd? is an excellent blog post with further information.

# CRI-O

CRI-O is an implementation of the Kubernetes Container Runtime Interface (CRI) that enables OCI-compatible runtimes. It is considered to be a lightweight alternative to using Docker and containerd. CRI-O also supports running Kata Containers (a virtual-machine-like container created by VMware) and is extensible to any other OCI-compliant runtime. This makes it an attractive runtime to use for a number of use cases.

# Docker

Not to be confused with the complete Docker platform, Docker actually contains a runtime engine which ironically is essentially containerd. Kubernetes supported Docker as a container runtime until the v1.20 release of Kubernetes.

# Containers

The container instance is the deployment and operation of software as defined by a container specification. The container runs the defined software (usually provided by an image) contained by a number of settings and limits.

# Operating System

The operating system is the lowest of the constructs, which is what our containers run on. In most container runtimes, the kernel is shared among all container instances.