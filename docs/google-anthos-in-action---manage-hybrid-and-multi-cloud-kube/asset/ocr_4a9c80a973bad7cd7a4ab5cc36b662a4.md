[7] Access Transparency enabled services lets customers control access to organization’s data by Google personnel. It also provides logs that capture the actions Google personnel take when accessing customer’s content.

[8] https://cloud.google.com/anthos/multicluster-management/connect/registering-a-cluster

[9] Google Cloud Identity and Access Management (IAM), lets you give more granular access to specific Google Cloud resources and prevents unwanted access to other resources.

[10] Policy Controller is part of Anthos Configuration Management, allowing administrators to define customized rules to place guard-rails for security, resource management, or operational reasons. Policy Controller is explored in greater depth in Chapter 14 with Anthos Configuration Management.

[11] Binary Authorization is explored in further detail in Chapter 15: Anthos integration with CI/CD

[12] See Chapter 6 for details on SLIs, SLOs, and SLAs with Anthos

[13] the local file path where your kubeconfig containing an entry for the cluster being registered is stored. This defaults to $KUBECONFIG if that environment variable is set; otherwise, this defaults to $HOME/.kube/config.

[14] All accounts logging in to a cluster need to hold at least the following Kubernetes RBAC roles in the cluster:

View: Kubernetes primitive role that allows read-only access to see most objects in a namespace. It does not allow viewing roles or role bindings.

Cloud-console-reader: Users who want to view your cluster's resources in the console need to have the relevant permissions to do so. You define this set of permissions by creating a ClusterRole RBAC resource, cloud-console-reader, in the cluster. cloud-console-reader grants its users the get, list, and watch permissions on the cluster's nodes, persistent volumes, and storage classes, which allow them to see details about these resources.