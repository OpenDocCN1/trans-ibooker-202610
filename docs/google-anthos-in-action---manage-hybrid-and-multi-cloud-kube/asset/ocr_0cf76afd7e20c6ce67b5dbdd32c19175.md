+   - A service account used to maintain a connection between the cluster and Google Cloud with the GKE Connect Agent IAM role.
+   - A service account used to send logs and metrics to Google Cloud's operations suite with the following IAM roles:
+     - Logs Writer
+     - Monitoring Metric Writer
+     - Stackdriver Resource Metadata Writer
+     - Monitoring Dashboard Configuration Editor
+     - Ops Config Monitoring Resource Metadata Writer

If you want to enable the above described APIs and create the needed GCP Service Accounts during installation using the bmctl tool, the account used for installation must have either the Project Owner/Editor role or, at least, the following roles assigned:

+   - Service Account Admin
+   - Service Account Key Admin
+   - Project IAM Admin
+   - Compute Viewer
+   - Service Usage Admin

Finally, there is one more requirement to cover that Anthos will use for cluster metrics.

# Cloud metric requirements

In order to send metrics to Google Cloud's operations suite, in addition to the service account listed above, a Cloud Monitoring Workspace must be created within the GCP project.

# 21.4 Creating Clusters

After all the requirements have been satisfied you can proceed with the cluster creation.

The following sections assume that all the installation tasks are performed from a machine that satisfies the requirements stated in the Admin