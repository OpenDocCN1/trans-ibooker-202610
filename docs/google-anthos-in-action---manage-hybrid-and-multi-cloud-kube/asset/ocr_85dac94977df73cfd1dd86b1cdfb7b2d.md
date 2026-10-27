Anthos Config Management will be key to satisfy this requirement:

# Anthos Config Management

Anthos Config Management capabilities are described extensively in the Anthos Config Management chapter. Here we recall a couple of them that are especially useful for managing applications and Network Functions deployments on a large fleet of clusters:

+   - Multiple repository mode
+   - Cluster Selectors

# Multiple repository mode

Enabling multi repository mode on Anthos Config Management allows to sync configuration from multiple repositories to the same set of clusters. A single **root repository**, typically managed by a central platform team, hosts cluster and centrally defined namespace scoped configurations, while optional **namespace repositories** are used to configure objects in specific namespaces. This capability extends ACM usage, beyond platform configuration and policies management, to the deployment of applications: it is possible to delegate the setup and control of a namespace repository to an application release team. Centrally defined namespaces resources are inherited while the application team is free to configure application related ones (deployments, config maps etc..), if there are conflicts between root and the namespace repository only the declaration in the root repository is applied to the cluster.

Figure 10.7: Anthos Config Management with multiple repositories