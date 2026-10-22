+   - A ServiceAccount can also have an additional type of secret known as an `imagePullSecret`. These secrets are used when an application needs to use images from a private registry.
+   - By default a ServiceAccount does not have any access to the Kubernetes API. To add this you would create a RoleBinding.

See the Kubernetes documentation for more details on [managing ServiceAccounts](https://) and configuring them for [role-based access](https://).

# Secrets

As you learned in Chapter 2, a Secret provides secure access to information you need to keep private. Your `mysql` Helm release contains a Secret called `mysql` containing login credentials for the MySQL instances themselves:

```
apiVersion: v1
kind: Secret
metadata:
  name: mysql
  namespace: default
  labels: ...
type: Opaque
data:
  mysql-root-password: "VzhyNEhIcmdTTQ=="
  mysql-password: "R2ZtNkFHNDhpOQ=="
  mysql-replication-password: "bDBiTWVzVmVORA=="
```

The three different passwords represent different types of access: the `mysql-root-password` provides administrative access to the MySQL node, while the `mysql-replication-password` is used for nodes to communicate for the purposes of data replication between nodes. The `mysql-password` is used by client applications to access the database to write and read data.

# ConfigMaps