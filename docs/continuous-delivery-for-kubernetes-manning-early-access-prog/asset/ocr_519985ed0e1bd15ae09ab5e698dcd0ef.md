This section covers what Crossplane needs in order to work and how Crossplane components will manage our CompositeResources.

First of all, it is important to understand that you will need to install Crossplane in a Kubernetes cluster. This can be the Cluster where your applications are running or a separate cluster where Crossplane will run. This cluster will have some Crossplane components that will understand our CompositeManagedResources and have enough permissions on the cloud platform to provision resources on our behalf. You might be wondering if you can install Crossplane into a KIND Cluster, and the answer is yes, but Crossplane really shines if you have access to a Cloud Provider. For that reason, we will use Google Cloud Platform in the next sections.

![](bbox=[0.13, 0.35, 0.88, 0.68])

Figure 4.9 Crossplane in Google Cloud Platform

The previous figure shows Crossplane installed inside a Kubernetes Cluster, with the Crossplane GCP provider installed and configured to use a Google Cloud Platform account with enough rights to provision PostgreSQL and Redis instances.

Each of the Crossplane Providers available requires a specific security configuration to work and an account inside the Cloud Provider where we want to create resources.

Once a Crossplane Provider is installed and configured, in this case, the GCP provider we can start creating resources that are managed by this provider. You can find the resources offered by each provider in the following documentation site: https://doc.crds.dev/github.com/crossplane/provider-gcp