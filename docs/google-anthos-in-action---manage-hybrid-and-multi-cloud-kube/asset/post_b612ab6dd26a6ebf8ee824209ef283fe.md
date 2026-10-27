
![](img/fb3bebce7b5f593bca1a7f8a1371be06_635_0.png)

As of January 2022, a smaller set (45 solutions) have been tested against the GKE on-prem environment. We can view the solutions for on-prem by adding the appropriate filter “*deployment-env: gke-on-prem*”, as shown in Figure 2, to the search options. As of January 2022, 78 solutions have been tested against Anthos environments, as shown in figure 3.

**Figure 17.3: Solutions available for Anthos GKE-on-prem environment.**

## Marketplace

Back to Marketplace page

Your solutions

Your orders

Filter Type to filter

## Category

- Big data (4)
- Analytics (3)
- Databases (14)
- Machine learning (3)
- Developer tools (12)
- Compute (4)
- Blog & CMS (4)
- Security (4)
- Developer stacks (2)
- Networking (3)

## Type

Kubernetes apps

## Deployment Environment

GKE on-prem

45 results

Search Marketplace

aqua
Aqua Enterprise Security Platform
Aqua Security Software Inc.
Aqua provides a cloud-native security platform on Kubernetes
Anthos

Bitpoke App
Bitpoke
WordPress on Kubernetes
Anthos

Cassandra Cluster
Google Click to Deploy containers
NoSQL database with masterless replication for high availability
Anthos

paloalto
CN Series NGFW
Palo Alto Networks, Inc.
Container firewalls secure Kubernetes environments
Anthos

Consul
Google Click to Deploy containers
Consul is a distributed service mesh to connect, secure and configure services
Anthos

Crunchy PostgreSQL for GKE
Crunchy Data
Trusted open source PostgreSQL-as-Service on GKE
Anthos

etcd
etcd
Google Click to Deploy containers
etcd cluster installation with HA
Anthos

Flink Operator
Google Click to Deploy containers
Lightning-fast unified analytics engine
Anthos

Gitlab
Google Click to Deploy containers
GitLab is a single application for the complete DevOps lifecycle
Anthos

While browsing the solutions in the marketplace, you can identify third party solutions that are compatible with Anthos by looking for the “Works with Anthos” logo attached to the listing. In figure 4, you can see solutions that

have a small Anthos button - this button is shown to showcase the solutions that have been certified to work with Anthos.

These listings conform to the requirements of the **Anthos Ready Program** ([https://cloud.google.com/anthos/docs/resources/anthos-ready-partners](https://cloud.google.com/anthos/docs/resources/anthos-ready-partners)), which identify partner solutions that adhere to Google Cloud’s interoperability requirements and have been validated to work with the Anthos platform in order to meet the infrastructure and application development needs of enterprise customers. To qualify, partner-provided solutions must complete, pass, and maintain integration requirements to earn the Works with Anthos badge.

**Figure 17.4: Solutions available for Anthos environment.**

You can access Private Catalog via Cloud console Navigation Menu under Tools (see Fig. 6). From there it is possible to create new private marketplaces, add applications, and configure access rights. Each Private Catalog should be hosted by a GCP project and it is possible to add Catalog IAM permissions at the Folder and Project level. Sharing a catalog with a GCP Org, Folder or Project allows customers to share their solutions with their end users. The steps are very intuitive and the interested reader can find more information online[^1].

Figure 17.6: Accessing Private Catalog.

![](img/fb3bebce7b5f593bca1a7f8a1371be06_640_0.png)

### 17.1.3 Deploying on a GKE on-prem cluster

If your intent is to deploy solutions from Marketplace to an Anthos GKE on-prem cluster then you need to define one or more namespaces on the target clusters and annotate the namespace with a secret which will allow you to deploy the chosen solutions. There are a number of steps required:

- 1. If your cluster runs Istio, then any external connection to third-party services are blocked by default so it’s important to configure Istio egress traffic to allow connection to the external OS package repository (see Chapter 5, Anthos Service Mesh: security and observability at scale).
- 2. You need to allow the downloading of images from the Google

Container Registry by creating a firewall or proxy rule which allows access to marketplace.gcr.io.

- 3. In your GKE on-prem cluster, you might need to create a Google Cloud service account. This can be done via a cloud shell like this:

```
gcloud iam service-accounts create sa-name \
  --description="sa-description" \
  --display-name="sa-display-name"
```

- 4. You need to sign in to your Anthos GKE on-prem cluster using a token or credentials for a Kubernetes Service Account (KSA) with Kubernetes "cluster-admin" role. (Roles were discussed in Chapter 4, Anthos, the computing environment built on Kubernetes) This would allow you to have super-user access to perform any action on any resource. From the console, you can generate a new public/private key pair downloaded to your machine by running the command:

```
gcloud iam service-accounts keys create ~/key.json \
  --iam-account sa-name@project-id.iam.gserviceaccount.com
```

- 5. If you are deploying from the Cloud Marketplace for the first time, you need to create an application-system namespace in your cluster by running the command:

```
kubectl create namespace application-system
```

- 6. Then, you need to create the Kubernetes Secret that contains an imagePullSecret for application-system, using the following command:

```
JSON_KEY_FILENAME=path_to/service_account_key.json
IMAGEPULLSECRET_NAME=gcr-json-key
kubectl create secret docker-registry $IMAGEPULLSECRET_NAME \
  --namespace="application-system" \
  --docker-server=gcr.io \
  --docker-username=_json_key \
  --docker-password="$(cat $JSON_KEY_FILENAME)"
```

- 7. The next step is to apply the imagePullSecret to the default Service Account in the application-system namespace by running the command:

```
kubectl patch sa default -n application-system -p ""imagePullSecr
```

8. Finally, for each namespace that you want to deploy an application to, you must create a new Secret and annotate the namespace with that Secret with the following command:

```
JSON_KEY_FILENAME=path_to/service_account_key.json
IMAGEPULLSECRET_NAME=gcr-json-key
kubectl create secret docker-registry $IMAGEPULLSECRET_NAME \
  --namespace=$NAMESPACE_NAME \
  --docker-server=gcr.io \
  --docker-username=_json_key \
  --docker-password="$(cat ~/$JSON_KEY_FILENAME)"
```

```
kubectl annotate namespace $NAMESPACE_NAME marketplace.cloud.goog
```

9. Once $NAMESPACE_NAME is defined as explained in the previous step, this namespace can be used to install on your on-prem cluster.

In other words, deploying on Anthos whether on GCP or on prem is just a matter of defining the appropriate namespace on your target Kubernetes clusters and annotating the namespace with the secret keys needed for pulling images for your solutions.

In the next section, we describe how to install a number of predefined solutions belonging to different categories including modernization tools, databases, monitoring and logging, CI/CD, productivity and machine learning.

### 17.2 Real world scenarios

In this section, we briefly describe how to deploy Marketplace solutions available for Anthos. Our examples are intentionally heterogeneous and belong to different categories. Please note we have already deployed Migrate for Anthos in Chapter 18, Migrate for Anthos, from Cloud Marketplace. Therefore we will skip it here but it is a very solid use case. Let’s see a few additional examples.

### 17.2.1 Example 1: ElasticSearch

instance name elasticsearch-1 and 2 replicas.

# Figure 17.8: Deploying an ElasticSearch solution

![](img/fb3bebce7b5f593bca1a7f8a1371be06_644_0.png)

Creating the clusters might take a few minutes (see Fig. 9) which is always a good reason to have a coffee break and let Marketplace do all the work on our behalf.

# Figure 17.9: Creating a cluster for the new ElasticSearch solution
