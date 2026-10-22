# MYSQL HELM CHART CONFIGURATION OPTIONS

If you examine the default values.yaml file provided with the Bitnami MySQL Helm chart, you’ll see that there are quite a few options available beyond the simple selections shown here. The configurable values include the following:

+   - Images to pull and their locations
+   - The Kubernetes StorageClass that will be used to generate PersistentVolumes
+   - Security credentials for user and administrator accounts
+   - MySQL configuration settings for primary and secondary replicas
+   - Number of secondary replicas to create
+   - Details of liveness, readiness probes
+   - Affinity and anti-affinity settings
+   - Managing high availability of the database using pod disruption budgets

Many of these concepts you’ll be familiar with already, and others like affinity and pod disruption budgets will be covered later in the chapter.

Once you’ve created the values.yaml file, you can start the cluster using this command:

```
helm install mysql bitnami/mysql -f values.yaml
```

After running the command you’ll see the status of the install from Helm, plus instructions that are provided with the chart under NOTES:

```
NAME: mysql
LAST DEPLOYED: Thu Oct 21 20:39:19 2021
NAMESPACE: default
STATUS: deployed
REVISION: 1
TEST SUITE: None
NOTES:
...
```