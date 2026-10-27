
As a result, you should see something like this:

```
NAME                 TYPE         CLUSTER-IP       EXTERNAL-IP      PORT
app-source-vm        ClusterIP    None             <none>           <none>
kubernetes           ClusterIP    10.63.240.1      <none>           443
my-service           LoadBalancer 10.63.243.209    35.232.24.49     80:
```

In this case, the external IP address is 35.232.24.49. Now I can open a browser and check that everything is ok (see Figure 18.5).

![](img/fb3bebce7b5f593bca1a7f8a1371be06_685_0.png)

Congratulations! You have successfully migrated a virtual machine running on GCE into a container running on GKE using the CLI. If you want to see another example of basic migration, I would suggest you consider the hands-on lab “Migrate for Anthos: Qwik Start” available on quicklabs[27].

Now that you know how to execute a migration using the CLI, we will move on to the next section where we will use the Cloud console UI to perform a migration.

## 18.4.3 Google Cloud Console UI migration example

In this section, we use M4A to migrate an application running as a virtual machine on Google Compute Engine to Google Kubernetes Engine. The GKE processing clusters can be located in the cloud or on-prem. The migration will be run through the Graphical User Interface (GUI) available

via the Google Cloud Console. Note that the migration process is consistent between the CLI and the GUI.

A GKE cluster will be used as a “processing cluster” to control the migration. The artifacts generated during the process will be stored on Google Cloud Storage (GCS) and the final container images are pushed on Google Container Registry. Under the hood this is identical as for the CLI-driven migration.

The first step is to access Anthos Migrate from console (see Fig.6) at

[https://console.cloud.google.com/anthos/migrate](https://console.cloud.google.com/anthos/migrate).

### Figure 18.6: Accessing Anthos Migrate to containers

![](img/fb3bebce7b5f593bca1a7f8a1371be06_686_0.png)

For the sake of simplicity, we will deploy a VM from Marketplace with Tomcat Server preinstalled. We will use this workload to migrate Tomcat from the VM to a container.

Let’s start by accessing the Google Click-to-Deploy repository with Tomcat(see Figure 18.7). The URL is:

https://console.cloud.google.com/marketplace/details/click-to-deploy-images/tomcat.

### Figure 18.7: Deploying a Tomcat application to VMs

![](img/fb3bebce7b5f593bca1a7f8a1371be06_687_0.png)

Then, let's select the zone where we want to deploy (see Figure 18.8):

### Figure 18.8: Deployed Tomcat application

![](img/fb3bebce7b5f593bca1a7f8a1371be06_688_0.png)

Once the VM with Tomcat Server is deployed it is possible to access the web site from an external IP (see Fig 9).

### Figure 18.9: Tomcat solution deployed with Google Click-to-Deploy

![](img/fb3bebce7b5f593bca1a7f8a1371be06_689_0.png)

### Tomcat

Solution provided by Google Click to Deploy

| Site address | http://35.238.48.196/ |
| Admin user | tomcat |
| Admin password (Temporary) | EKDm4yrQ |
| Instance | tomcat-vm-gulli-vm |
| Instance zone | us-central1-c |
| Instance machine type | n1-standard-1 |

In this deployment the IP address is http://35.238.48.196/, so if you access the web site, you should see the output shown in Figure 18.10:

### Figure 18.10: Accessing Tomcat from Web

# It works !

The next step is to start the proper migration process. The first thing to do is to create a "processing cluster", a cluster which will be used to control the migration of our source VM. You can perform this task by accessing the "Migrate to containers" Menu on Anthos and use the “Add Processing Cluster” option (see Figure 18.11):

### Figure 18.11: Starting the migration process

### ADD PROCESSING CLUSTER

It is convenient to follow the suggestion provided in the GUI and create a new cluster dedicated to processing (see Figure 18.12):

### Figure 18.12: Choosing a name for the “processing cluster”

![](img/fb3bebce7b5f593bca1a7f8a1371be06_690_0.png)

Once the cluster is ready, you should be able to see it via the Google Cloud Console in the GKE section (see Figure 18.13).

### Figure 18.13: The processing cluster is ready for use

### 2 Define source name and type

Define a name for the new source.

Name *
migrate-vm-tomcat-gulli

Select the source environment from which you want to migrate workloads.

Compute Engine

Learn how to define other source types (VMWare, AWS and Azure) supported currently through the tool's CLI. Learn more

Once you have chosen a name, you can select the project in which the source VMs are placed (see Figure 18.19)

### Figure 18.19: Selecting the project in which the source VMs are placed

### Setup migration

Migration name *
tomcat-migr

Select Source *
migrate-vm-tomcat-gulli processing-cluster-migrate

To learn how to add more sources click here .

VM OS type *
Linux

Select the OS type (Windows or Linux) of the VM candidate for migration

VM ID *
tomcat-vm-gulli-vm

Specify the ID of the migrated VM.

Migration Intent *
Image

Then, M4A will start generating the migration plan (see Figure 18.22):

### Figure 18.22: Generating the migration plan

| Migration name ↑ | VM ID | Status |
|---|---|---|
| tomcat-migr | tomcat-v... | Generating migration plan |

During the migration, we can check the progress made with the following command, which will dump an extensive debug:

```
migctl migration status my-migration -v
```

Once the migration plan is generated (see Figure 18.23), it is possible to inspect the results with the GUI:

**Figure 18.23: An overview of the migration plan generated by M4A**

![](img/fb3bebce7b5f593bca1a7f8a1371be06_696_0.png)

In particular, the option menu allows us to edit the generated migration plan (see Figure 18.24):

**Figure 18.24: Reviewing and editing the generated migration plan**

![](img/fb3bebce7b5f593bca1a7f8a1371be06_696_1.png)

Let's have a look at what has been generated by editing the migration plan (see Figure 18.25):

**Figure 18.25: Editing the generated migration plan**

### Edit migration plan

```
```yaml
apiVersion: anthos-migrate.cloud.google.com/v1beta2
kind: GenerateArtifactsFlow
metadata:
  creationTimestamp: null
  labels:
    migration: kuwflyjr
    migration-name: tomcat-migr
    migration-namespace: v2k-system
  name: generate-artifacts-flow-f3d851ee-2b23-48ad-88a8-6f21356d8918
  namespace: v2k-system
  ownerReferences:
  - apiVersion: anthos-migrate.cloud.google.com/v1beta2
    blockOwnerDeletion: true
    controller: true
    kind: Migration
    name: tomcat-migr
    uid: f3d851ee-2b23-48ad-88a8-6f21356d8918
spec:
# Review and set which artifacts to generate.
```
```

Normally, you don’t need to change the migration plan. However, this is useful if you either need to strip out unneeded VM components or you need to add some additional configuration. After checking, and if necessary, editing the migration plan, we can start generating the artifacts (see Figure 18.26):

**Figure 18.26: Generating artifacts with the edited migration plan**

![](img/fb3bebce7b5f593bca1a7f8a1371be06_698_0.png)

Once the migration plans are generated, they can be inspected by accessing the “artifacts” tab from the Google Cloud Console. This includes the Dockerfile, the Container image for deployment (that can be directly deployed), the Container image base layer (the non-runnable image layers), the Deployment spec YAML, the Migration plan YAML and the Artifact links YAML (see Figure 18.27):

### Figure 18.27: Generated Migration artifacts

![](img/fb3bebce7b5f593bca1a7f8a1371be06_698_1.png)

The generated artifacts are stored in Google Cloud Storage (GCS) with separated buckets for base and image layers (see Figure 18.28):

### Figure 18.28: Artifacts stored in GCS

![](img/fb3bebce7b5f593bca1a7f8a1371be06_699_0.png)

Let’s have a look at the system image with the Dockerfile, the deployment spec, the manifest and the migration YAML (see Figure 18.29):

**Figure 18.29: Dockerfile, deployment_spec, manifest, migration files**

| Name | Size | Type |
|---|---|---|
| Dockerfile | 700 B | text/plain; charset=utf-8 |
| deployment_spec.yaml | 1.7 KB | text/plain; charset=utf-8 |
| manifest.yaml | 735 B | text/plain; charset=utf-8 |
| migration.yaml | 3.1 KB | text/plain; charset=utf-8 |

In addition, images generated for migration are automatically pushed to the Google Container Registry (GCR) and you can browse them via the console (see Figure 18.30):

**Figure 18.30. Images generated for migration and pushed to GCR**
