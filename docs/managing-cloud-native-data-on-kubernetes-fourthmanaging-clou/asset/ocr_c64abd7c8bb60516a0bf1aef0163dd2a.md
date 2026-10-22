```
# don’t execute me yet!
helm install mysql bitnami/mysql
```

If you execute this command, it will create a release called `mysql` using the Bitnami MySQL Helm chart with its default settings. As a result you’d have a single MySQL node. Since you’ve already deployed a single node of MySQL manually in Chapter 3, let’s do something a bit more interesting this time and create a MySQL cluster. To do this you’ll create a `values.yaml` file with contents like this (or reuse the [sample]( ) provided in the source code):

```
architecture: replication
secondary:
  replicaCount: 2
```

The settings in this `values.yaml` file let Helm know that you want to use options in the Bitnami MySQL Helm chart to deploy MySQL in a replicated architecture in which there is a primary node and two secondary nodes.