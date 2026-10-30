**Config Connector** allows you to provision and manage GCP resources using Kubernetes manifests. This way, you gain a single API to provide both GCP and Kubernetes resources:

![](bbox=[0.12, 0.16, 0.85, 0.5])

Figure 8.11 – Anthos Config Connector

Config Connector comes with **Custom Resource Definitions (CRDs)** and associated controllers for almost all GPC services. If you want to learn more about the resources supported, visit https://cloud.google.com/config-connector/docs/reference/overview.

The Config Controller service, at the time of writing this book, is in a preview state. It is a Google-hosted service that combines the Config Connector and Config Sync services to allow GitOps operations for Google Cloud and Anthos resources. To achieve this, an administrator defines the desired state of the resources using Config Connector CRDs and stores them in a Git repository. Config Sync makes sure the desired state is enforced.

> **Important Note**
> It is possible that rebranding of this service might happen without any notice, so please refer to the link: https://cloud.google.com/anthos-config-management/docs/concepts/config-controller-overview to learn about current status of the service.