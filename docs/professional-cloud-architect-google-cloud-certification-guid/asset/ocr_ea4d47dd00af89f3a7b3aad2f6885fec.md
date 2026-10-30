4. Provide the cluster details; that is, the location and the number of nodes needed. We can also set up replication if needed. Note that we can also select our storage type – either SSD or HDD. This will default to SSD if we do not change it. Click Done to accept the cluster changes and click Create to complete the instance configuration:

# Select a cluster ID

ID is permanent

Cluster ID *
cloudarchitect-c2

# Select a location

Choice is permanent. Determines where cluster data is stored. To reduce latency and increase throughput, store your data near the services that need it. Learn more

Region *
europe-west2 (London)
Zone *
Any

# Allocate nodes

Node count can be updated at any time to meet your cluster's need for data throughput, storage, and rows read per second. For better instance performance, keep your cluster's CPU utilization under the recommended threshold for your app profile routing policy. Contact us if you need to increase your node quota. Learn more

Nodes *
3

Figure 12.16 – Adding a cluster

Once we have our instance and clusters, we can create a table. We touched on this earlier, but Bigtable has a command-line tool called cbt, and creating a table is simplistic when we use it. Once again, we will make assumptions that you are familiar with this tool. Open Cloud Shell so that we can configure the cbt tool. We will look at cbt and Cloud Shell in more detail in Chapter 16, Google Cloud Management Options:

+   1. Before we create a table, run the following syntax to mitigate the need to specify them in future command lines:

```
echo project = <project ID> ~/.cbt rc
echo instance = <instance ID> >> ~/.cbt rc
```