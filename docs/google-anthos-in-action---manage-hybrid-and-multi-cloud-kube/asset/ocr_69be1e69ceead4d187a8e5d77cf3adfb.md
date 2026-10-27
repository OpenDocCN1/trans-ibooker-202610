Where _CLUSTER_NAME_ is the name you want to give to the cluster and _CLOUD_PROJECT_ID_ is the project id of the project you want to use with Anthos on bare metal.

If you haven’t already enabled the required APIs you can add the `--enable-apis` option to the above command to enable them, and if you haven't created the required service accounts previously you can add the option `--create-service-accounts` to have them created along with the needed roles.

# Populate the cluster configuration file

Before creating the cluster we need to properly prepare the configuration file created by bmctl, the file is saved by default in a folder named with cluster name inside a folder named _bmctl-workspace_.

In this section, we will explain the options in the configuration file.

# SSH Private Key

This is the SSH private key that will be used to connect to the nodes during the cluster deployment.

Add the `sshPrivateKeyPath:` spec the full path to access an SSH private key authorized to access all the target nodes as root, for example:

```
sshPrivateKeyPath: /root/.ssh/anthoskey
```

# GCP Service Accounts Keys

If you manually created the GCP service accounts before running bmctl you need to populate the related fields with the full paths to service account keys as in the example below, if you used the bmctl `--create-service-accounts` parameter they will be already populated.

```
gkeConnectAgentServiceAccountKeyPath: /root/bmctl-workspace/.sa-k
gkeConnectRegisterServiceAccountKeyPath: /root/bmctl-workspace/.s
cloudOperationsServiceAccountKeyPath: /root/bmctl-workspace/.sa-k
```