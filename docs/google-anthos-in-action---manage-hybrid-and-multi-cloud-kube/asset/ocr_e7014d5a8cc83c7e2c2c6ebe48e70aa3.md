via the Google Cloud Console. Note that the migration process is consistent between the CLI and the GUI.

A GKE cluster will be used as a “processing cluster” to control the migration. The artifacts generated during the process will be stored on Google Cloud Storage (GCS) and the final container images are pushed on Google Container Registry. Under the hood this is identical as for the CLI-driven migration.

The first step is to access Anthos Migrate from console (see Fig.6) at

[https://console.cloud.google.com/anthos/migrate](https://console.cloud.google.com/anthos/migrate).

# Figure 18.6: Accessing Anthos Migrate to containers

![](bbox=[0.316, 0.411, 0.639, 0.891])