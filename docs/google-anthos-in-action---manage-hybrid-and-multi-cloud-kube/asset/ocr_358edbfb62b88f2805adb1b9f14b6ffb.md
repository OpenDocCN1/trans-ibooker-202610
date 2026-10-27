To create code repository run the below commands:

```
```bash
gcloud init
```
```

This will initialize your gcloud command line tool

```
```bash
gcloud source repos create [REPO_NAME]
```
```

This will create a repository of the name REPO_NAME

Now that the repository is ready to use it you need to choose one of the three
authentication methods:

+   - SSH
+   - Cloud SDK
+   - Manually generated credentials

Refer to the following link to see step by step guide how to do it:
[https://cloud.google.com/source-repositories/docs/authentication](https://cloud.google.com/source-repositories/docs/authentication)

Once the repository is set up and you have successfully authenticated, you
can interact with it using Git commands like: `git clone`, `git pull`, and
`git push`.

# 14.3.2 Artifact Registry

Artifact Registry is a next iteration of the Google Container Registry service.
On top of being able to store container images Artifact Registry can store
other packages like Maven[23], npm[24], and Python[25] with more capabilities
to come soon. The services are fully integrated with the Google Cloud
Platform ecosystem. This means you can control access to your artifacts
using IAM policies, access Cloud Source Repositories, trigger automatic
builds using Cloud Build and deploy to Google Kubernetes Engine, App
Engine and Cloud Functions.

Artifact Repositories can be created in regions closest to your workloads so
you can leverage the high speed Google network to pull your artifacts.

From the security standpoint your containers can be scanned for