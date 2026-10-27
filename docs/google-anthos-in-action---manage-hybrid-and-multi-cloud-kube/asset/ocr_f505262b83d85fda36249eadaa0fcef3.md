As a result, you should see something like this:

```
NAME                 TYPE         CLUSTER-IP       EXTERNAL-IP      POR
app-source-vm        ClusterIP    None             <none>           <no
kubernetes           ClusterIP    10.63.240.1      <none>           443
my-service           LoadBalancer 10.63.243.209    35.232.24.49     80:
```

In this case, the external IP address is 35.232.24.49. Now I can open a browser and check that everything is ok (see Figure 18.5).

![](bbox=[0.139, 0.339, 0.819, 0.544])

Congratulations! You have successfully migrated a virtual machine running on GCE into a container running on GKE using the CLI. If you want to see another example of basic migration, I would suggest you consider the hands-on lab “Migrate for Anthos: Qwik Start” available on quicklabs[27].

Now that you know how to execute a migration using the CLI, we will move on to the next section where we will use the Cloud console UI to perform a migration.

# 18.4.3 Google Cloud Console UI migration example

In this section, we use M4A to migrate an application running as a virtual machine on Google Compute Engine to Google Kubernetes Engine. The GKE processing clusters can be located in the cloud or on-prem. The migration will be run through the Graphical User Interface (GUI) available