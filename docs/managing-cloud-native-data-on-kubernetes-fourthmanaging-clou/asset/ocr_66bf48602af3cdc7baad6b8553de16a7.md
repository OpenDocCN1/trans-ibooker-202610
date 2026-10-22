```
spec:
  ...
  serviceAccountName: mysql
  containers:
    - name: mysql
      image: docker.io/bitnami/mysql:8.0.26-debian-10-r60
      volumeMounts:
        - name: data
          mountPath: /bitnami/mysql
        - name: config
          mountPath: /opt/bitnami/mysql/conf/my.cnf
          subPath: my.cnf
  volumes:
    - name: config
      configMap:
        name: mysql-primary
```

Mounting the ConfigMap as a volume in a container results in the creation of a read-only file in the mount directory which is named according to the key and has the value as its content. For our example, mounting the ConfigMap in the pod’s mysql container results in the creation of the file /opt/bitnami/mysql/conf/my.cnf.

This is just one of several ways that ConfigMaps can be used in Kubernetes applications:

+   - As described in the Kubernetes documentation, you could choose to store configuration data in more granular key-value pairs, which also makes it easier to access individual values in your application
+   - You can also reference individual key-value pairs as environment variables you pass to a container
+   - Finally, applications can access ConfigMap contents via the Kubernetes API