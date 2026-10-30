# NFS

Some people might be confused about NFS and another protocol used to share files over the network, Server Message Block (SMB). Although SMB and NFS both work with various OSs (e.g., Windows, Linux, macOS, etc.), the reality is that SMB is most often used by Windows and macOS systems, while NFS is more often used by Linux and Unix systems. ESXi does not support SMB.

Initially, ESXi only supported NFS v3, but it later gained support for NFS v4.1. The major difference between these versions is that v4.1 supports having multiple network connections, or multipathing. This setup allows multiple IP addresses to access the same NFS share, thereby increasing both redundancy and performance.

# Using NFS with ESXi

In the early days of ESXi, NFS was by far the most popular shared storage method. This was not surprising as NFS has been around since the 1990s and has proven to be an extremely robust, reliable, and efficient mechanism to deliver files between different systems. All modern operating systems support NFS, and it is supported by a range of systems, from Android to mainframes.

Production environments use storage arrays or appliances connected to hosts via a high-bandwidth network. As it is unlikely that you will have access to equipment like this, in this demonstration I will show how to use Windows and Linux machines for the storage and delivery of the VMs over a 1GbE network.