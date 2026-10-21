# AZURE AUTOSCALING

Azure provides autoscaling as a feature in a number of its products, including virtual machine scale sets and Azure functions. The autoscaling features allow you to efficiently scale your resources up and down when appropriate. With virtual machine scale sets, there can be up to a 10-minute delay for the new virtual machines to be available when scaling out.

# Availability and Security

Etcd is a control plane service, so it is important that it is highly available and that the communications to it are secure. We recommend that you run at least three etcd nodes in a cluster to ensure availability of the cluster.
Cluster throughput will be dependent on storage performance. We strongly recommend against using static discovery mechanisms for your cluster nodes, and instead to use DNS SRV records per the etcd clustering guide.

# Etcd TLS

Etcd supports client-to-server and peer (server-to-server) Transport Layer Security (TLS) encryption. We strongly recommend that you enable this as etcd is a control plane system. We won’t go into implementation details here (as they are highly dependent on your infrastructure). You can find configuration options in the etcd security documentation.

# Role-based access control

Since etcd v2.1, role-based access control (RBAC) has been a feature of the etcd API. Using RBACs, you can create access patterns that only allow certain users (or applications) access to a subset of the etcd data.
Traditionally, this is done by providing HTTP basic authentication when making HTTP requests to etcd. As of etcd v3.2, if you’re using the --client-cert-auth=true configuration option, the Common Name (CN) of the client TLS certificate will be used as the user (instead of a username/password combination).