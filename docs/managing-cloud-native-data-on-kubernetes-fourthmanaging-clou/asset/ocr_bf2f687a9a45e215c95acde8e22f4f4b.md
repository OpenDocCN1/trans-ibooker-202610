```
helm temp
late mysql bitnami/mysql -f values.yaml
```

Running this command will produce quite a bit of output, so you may want to pipe it to a file (append “> values-template.yaml” to the command) so you can take a longer look. Alternatively, you can look at the copy we have saved in the source code repository.

You’ll notice that there are several different types of resources created, as summarized in Figure 4-3. Many of the resources shown have been discussed above, including the StatefulSets for managing the primary and secondary replicas, each with its own service (the chart also creates headless services which are not shown in the figure). Each pod has its own PersistentVolumeClaim which is mapped to a unique Persistent Volume.

Figure 4-3 also includes resource types we haven’t discussed previously. Notice first that each StatefulSet has an associated ConfigMap that is used to provide a common set of configuration settings to its pods. Next, notice that there is a Secret named mysql, which stores passwords needed for accessing various interfaces exposed by the database nodes. Finally, there is a ServiceAccount resource, which is applied to every pod created by this Helm release.