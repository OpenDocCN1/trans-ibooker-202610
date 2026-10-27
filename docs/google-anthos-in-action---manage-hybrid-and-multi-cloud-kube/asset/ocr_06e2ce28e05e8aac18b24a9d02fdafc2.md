Now that we know how Cloud Deploy interacts with the other Anthos CI/CD tools lets see how we can configure it for Continuous Delivery of the Anthos applications.

# 14.4.2 Google Cloud Deploy delivery pipeline for Anthos

Cloud Deploy uses delivery pipelines manifests that define the promotion sequence to describe the order in which to deploy to targets. The targets are described in the pipelines definition or in separate files. The Saffold configuration file is used to render and deploy the Kubernetes resource manifest. Figure x defines the components that and flow that Cloud Deploy uses.

# Figure 14.36 Cloud Deploy workflow

![](bbox=[0.154, 0.434, 0.862, 0.81])

Let's have a look at the sequences of action needed to configure continuous delivery with Cloud Build.