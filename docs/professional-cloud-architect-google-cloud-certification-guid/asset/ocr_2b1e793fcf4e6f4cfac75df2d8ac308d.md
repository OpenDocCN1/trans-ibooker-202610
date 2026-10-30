# Service settings

A service exposes a unique endpoint and automatically scales the underlying infrastructure to handle incoming requests. Deployment platform and service name cannot be changed.

# Deployment platform

Cloud Run (fully managed)
Cloud Run for Anthos

You don't have any clusters with Cloud Run for Anthos enabled. Create an Anthos GKE cluster to start. Learn more
CREATE CLUSTER

Available Anthos GKE clusters *
cloudrun-default-cluster (will be created) us-central1-a
Cluster for this service can't be changed later. Learn more

Namespace
default

# Service name *

cloudrun-test

# Connectivity *

Internal
The service can only be invoked through the cluster network and is not accessible from the Internet.
External
The service can be invoked through the Internet, in addition to through the cluster network.

# Figure 8.19 – Cloud Run for Anthos

If you don't have any Anthos clusters with Cloud Run enabled, the cluster will be created for you. If you want to create a cluster with Cloud Run enabled, add the following flags to the gcloud command:

```
gcloud container clusters create CLUSTER_NAME \
  --addons=HttpLoadBalancing,CloudRun \
  --machine-type=n1-standard-2 \
  --enable-stackdriver-kubernetes
```