```
helm install app fmtok8s/fmtok8s-app
```

You should see the following output:

```
NAME: app
LAST DEPLOYED: Sat Aug 28 13:52:48 2021
NAMESPACE: default
STATUS: deployed
REVISION: 1
TEST SUITE: None
NOTES:
Cloud-Native Conference Platform V1

Chart Deployed: fmtok8s-app - 0.1.0
Release Name: app
```

Figure 2.10 Helm installed the chart fmtok8s-app version 0.1.0

`helm install` creates a Helm Release, which means that you have created an application instance, in this case, the instance is called `app`. With Helm, you can deploy multiple instances of the application if you want to. You can list Helm releases by running:

```
helm list
```

The output should look like this:

```
NAME NAMESPACE REVISION UPDATED STATUS CHART APP VERSION
app default 1 2021-08-28 13:52:48.748112 +0100 BST deployed fmtok8s-app-0.1.0 0.1.0
```

Figure 2.11 List Helm releases

> NOTE: If instead of using `helm install` you run `helm template` Helm will output the YAML files which will apply against the cluster. There are situations where you might want to do that instead of `helm install`, for example, if you want to override values that the Helm charts don't allow you to parameterize or apply any other transformations before sending the request to Kubernetes.

# VERIFYING THAT THE APPLICATION IS UP AND RUNNING

Once the application is deployed, containers will be downloaded to your laptop to run, and this can take a while. You can monitor the progress by listing all the pods running in your cluster, once again, using the `-owide` flag to get more information: