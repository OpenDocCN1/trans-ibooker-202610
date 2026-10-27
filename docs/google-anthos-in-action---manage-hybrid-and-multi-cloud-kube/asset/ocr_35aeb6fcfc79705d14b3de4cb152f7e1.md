ReadWriteOnce (default) or ReadWriteMany.

+   - **DISK_SIZE**: The size you want for the boot disk. The default value is `20Gi`.
+   - **DISK_CLASS**: The storage class of the boot disk. The default value is `local-shared`.
+   - **FILE_PATH**: The full path of the customized cloud-init file. Depending on the image, this may be required to gain console access to the VM after it is created.
+   - **CPU_NUMBER**: The number of CPUs you want to configure for the VM. The default value is 1.
+   - **IMAGE_NAME**: The VM image, which can be `ubuntu20.04` (default), `centos8`, or a URL of the image.
+   - **MEMORY_SIZE**: The memory size of the VM. The default value is `4Gi`.

If parameters are not specified the defaults are used.

Alternatively it’s possible to apply a manifest defining a VirtualMachine custom resource which also enables popular Gitops deployment methods (declarative & asynchronous management).

# Orchestration and Automation for Large Compute Fleets

One of the impacts of Edge and Radio Access Network CNFs deployments is the need to have a central control plane capable of managing a wider and more granularly distributed compute fleet, and, moreover, deploying a number of application instances that could be orders of magnitude bigger than what is typically deployed in traditional data centers or public clouds.

In some cases there will be the need to deploy instances of the same application on tens, hundreds, or also thousands of compute clusters, distributed in edge locations. Often the locations where the instances must be deployed will need to be selectively defined, based on specific criterias, also dynamically with very short notice to adapt to changing needs from network, monitored objects or application users.

Central configuration and policy management capabilities provided by