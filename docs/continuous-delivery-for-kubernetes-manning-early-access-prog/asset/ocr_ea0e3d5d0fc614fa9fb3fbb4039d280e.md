![](bbox=[0.17, 0.18, 0.95, 0.46])

Figure 4.11 Lifecycle of Managed Resources with Crossplane

The following points indicate the sequence observed in Figure 4.X:

+   1. First, we need to create a resource, we can use any tool to create Kubernetes resources, `kubectl` here is just an example.
+   2. If the resource that we created is a Crossplane Managed Resource, let’s imagine a CloudSQLInstance resource, the specific Crossplane Provider will pick it up and manage it.
+   3. The first step to execute when managing a resource will be checking if it exists in the infrastructure (that is in the configured GCP account). If it doesn’t exist the Provider will send a request for the resource to be created in the infrastructure. Depending on the properties set on the resource, for example, which kind of SQL database is required, the appropriate SQL database will be provisioned, imagine for the sake of the example that we have chosen a PostgreSQL database.
+   4. The Cloud Provider after receiving the request, if the resources are enabled, will proceed to create a new PostgreSQL instance with the configured parameters in the Managed Resource.
+   5. The status of the PostgreSQL will be reported back to the Managed Resource, which means that we can use `kubectl` or any other tool to monitor the status of the provisioned resources. Crossplane provider’s will keep these in sync.
+   6. When the database is up and running the Crossplane Provider will create a secret to store the credentials and properties that our applications will need to connect to the newly created instance
+   7. Crossplane will regularly check the status of the PostgreSQL instance and update the managed resource.