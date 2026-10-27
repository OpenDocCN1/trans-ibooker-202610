service. An example for how to create such notifications can be found here:
[https://cloud.google.com/cloud-build/docs/configuring-notifications/configure-smtp](https://cloud.google.com/cloud-build/docs/configuring-notifications/configure-smtp)

# Deploying to Anthos Google Kubernetes Engine

Deployment to Google Kubernetes Engine can be done using either `kubectl` or `gke-deploy` builder. Note that `gke-deploy`[^27] is basically a wrapper around kubectl that incorporates Google best practices to deploy Kubernetes resources. For example, it adds the label `app.kubernetes.io/name` to the deployed Kubernetes resources.

Bellow you can see an example use of gke-deploy builder to deploy to a GKE cluster:

```
steps:
...
# deploy container image to GKE
- name: "gcr.io/cloud-builders/gke-deploy"
  args:
  - run
  - --filename=[kubernetes-config-file]
  - --location=[location]
  - --cluster=[cluster]
```

In future we can expect other Cloud Builders like AnthosCLI that will make the experience even more unified.

# Deploy to Cloud Run and Cloud Run for Anthos

Cloud Build allows you to build your Cloud Run container image and then deploy it to either Cloud Run or Cloud Run for Anthos. In both cases you would first build and push the image using standard Docker builder:

```
steps:
# Build the container image
- name: 'gcr.io/cloud-builders/docker'
  args: ['build', '-t', 'us-central1-docker.pkg.dev/$PROJECT_ID/$
# Push the container image to a registry
- name: 'gcr.io/cloud-builders/docker'
```