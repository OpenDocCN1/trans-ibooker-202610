tenants can have their own projects for non-Anthos resources and they can all share a single Anthos GKE on GCP clusters. Using a shared VPC allows Anthos and non-Anthos resources to connect to each other.

# Figure 12.3 Shared VPC architecture in GCP

![](bbox=[0.126, 0.226, 0.879, 0.728])

# Multiple VPC

The two previous VPC implementations result in a flat network where all resources are provisioned in a single logical VPC. In some cases, security or regulatory restrictions may require the separation of resources into multiple VPCs. The company or organization may also want each team or product to