![](bbox=[0.155, 0.111, 0.837, 0.501])

The fig. shows the Google Cloud Deploy show the interfaces with the following services:

+   - **CI tooling** can call the Google Cloud Deploy API or CLI to create a release. Hence most of the CI tools are supported including Cloud Build
+   - **Cloud Build** is used to render the manifests and to deploy to the target runtime.
+   - **Skaffold** is used by Cloud Build to render and deploy the manifests. As a result the application is deployed.
+   - **Cloud Storage** stores rendering source and rendered manifests
+   - **Google Cloud's operations suite** collects and stores Audit Logs. of the Cloud Deploy service.
+   - **Pub/Sub** allows publishing Cloud Deploys messages to Pub/Sub topics. This can be used to integrate with external systems.
+   - **GKE and Anthos Clusters** are the Target runtimes to which Google Cloud Deploy deploys the applications. This is done with the use of skaffold apply, through Cloud Build, to your target runtime.