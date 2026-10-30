# Confidential VM service

Enable this option to encrypt data while in use. This will keep data encrypted in memory during processing without Google having access to the encryption keys:

![](bbox=[0.215, 0.195, 0.68, 0.24])

Figure 4.23 – Confidential VM service

# Container

Enable this option to deploy and launch a Docker container using a Container-Optimized OS (COS). Note you can only deploy one container for each VM instance. This is separate to Google Kubernetes Engine (GKE), which we will discuss in Chapter 6, Managing Kubernetes Clusters with Google Kubernetes Engine:

![](bbox=[0.215, 0.42, 0.71, 0.465])

Figure 4.24 – Container

The preceding figure shows the simple checkbox to deploy a container image.

# Deletion protection

Enable this to prevent your GCE instance from accidental deletion. Checking this box will set the deletionProtection property on the instance resource. Usually, only critical VM instances that need to stay running indefinitely would have this option checked. Even after this is checked, instances can still be reset, stopped, or even removed after a project is terminated. Only users who have compute.instance.create permissions can modify this flag to allow a protected VM to be deleted.

# Reservations

Reservations can be created for VM instances in specific zones using custom or predefined machine types. Use reservations to ensure resources are available for your workloads when you need them; however, as soon as you create a reservation, you will immediately begin paying for it until the reservation is deleted. Increases in demand should be considered before using reservations. This option allows you to select an existing reservation, create a new one, or use none.