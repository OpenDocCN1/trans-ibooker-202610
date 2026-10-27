be defined in the target kubeconfig file. If this flag is omitted, you are prompted to choose from the clusters that are specified in your authentication configuration file.

+   - **USER_NAME**: Optional. Is the username to use in the kubeconfig file, if omitted defaults to CLUSTER_NAME-anthos-default-user
+   - **AUTH_CONFIG_FILE_PATH**: Specifies the path of the authentication configuration file
+   - **CA_CERT_PEM_FILE**: specifies the path to a PEM certificate file from your CA, needed if the authentication configuration file is stored on an https server.
+   - **CLUSTER_KUBECONFIG**: Is the target kubeconfig file where the oidc id token is wit ten, if omitted defaults to the kubectl default location

The command will open the browser on the OIDC provider consent login page where you need to insert credentials.

Your kubeconfig file now contains an ID token that your kubectl commands will use to authenticate with the Kubernetes API server on your cluster.

# 21.4.2 Creating a user cluster

Once you've created an admin or hybrid cluster you can add user clusters to it. You do this by applying a new config file that contains only the Cluster and Node Pool custom resources manifests for the new cluster.

The high level steps to create the first cluster are:

+   1. Use bmctl to create a cluster config template file
+   2. Modify the config file with desired settings
+   3. Apply the config file with bmctl

The tasks that are executed to create a user cluster are a subset of what was already done for the creation of the first cluster. In the next section we will detail the configuration file to deploy the user cluster.

# Create the cluster configuration