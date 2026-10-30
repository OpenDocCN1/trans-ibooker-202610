```
zone: europe-west4-a
  machineType: https://www.googleapis.com/compute/v1/projects/primeval-nectar-318916/zones/europe-west4-a/machineTypes/g1-small
  disks:
  - deviceName: boot
    type: PERSISTENT
    boot: true
    autoDelete: true
    initializeParams:
      sourceImage: https://www.googleapis.com/compute/v1/projects/centos-cloud/global/images/centos-stream-8-v20210721
  networkInterfaces:
  - network: https://www.googleapis.com/compute/v1/projects/primeval-nectar-318916/global/networks/default
  accessConfigs:
  - name: External NAT
    type: ONE_TO_ONE_NAT
```

Now, let's save it and use this configuration file to deploy a VM. Again, we will use the Cloud Shell tool to execute this using the Deployment Manager command-line syntax:

```
gcloud deployment-manager deployments create <deployment name> --config <config file>
```

Here, <deployment name> is the name of your overall deployment name. Remember that the VM instance name is specified in our configuration file and that <config file> is the name of the configuration file we created previously. Let's look at an example of creating a new deployment called deployment01 using our vm001.yaml configuration file:

```
gcloud deployment-manager deployments create deployment01 --config vm001.yaml
```