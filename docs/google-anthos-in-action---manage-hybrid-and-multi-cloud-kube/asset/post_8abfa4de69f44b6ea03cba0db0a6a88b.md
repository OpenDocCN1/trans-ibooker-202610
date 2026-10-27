
# Click to Deploy on GKE

Deploy via command line

Your app will use compute instances managed in a logical grouping called a "cluster", which will be configured in a way that's great for getting started with Kubernetes. For more options, visit the Kubernetes engine cluster creation page.

Creating "cluster-3" in zone "us-central1-a"...

Creating •••

Once the cluster is created, Marketplace will start deploying the solution on our behalf (see Fig. 17.10).

Figure 17.10: Deploying application components for the ElasticSearch solution.

![](img/fb3bebce7b5f593bca1a7f8a1371be06_646_0.png)

When the deployment is successfully executed, you can see the deployed solution on your chosen cluster as shown in Fig. 11; pretty easy indeed. Marketplace saves you a lot of time and speeds up the administrative tasks needed to maintain your Anthos applications.

## Figure 17.11: ElasticSearch solution deployed

|  | elasticsearch-3-elasticsearch | OK | Stateful Set | 2/2 | default |

Once ElasticSearch is deployed from the marketplace, you can use Anthos to manage the cluster. In figure 12, you will see cpu, memory and disk details.

## Figure 17.12: CPU, Memory and Disk monitoring for Elasticsearch deployed from Marketplace

![](img/fb3bebce7b5f593bca1a7f8a1371be06_647_0.png)

### 17.2.2 Example 2: MariaDB

Let’s now focus on how to deploy MariaDB, a popular open-source database, which was forked from the more popular MySQL relational database management system after acquisition by Oracle in 2009. The solution (see Fig. 13) is available at:

https://console.cloud.google.com/marketplace/details/google/mariadb

Figure 17.13: Deploying a MariaDB solution

# MariaDB

Container Registry tag: 10.3

MariaDB (Google Click to Deploy containers)

Multi instance MariaDB installation with replication

CONFIGURE

# Overview

MariaDB is a community-developed fork of the MySQL relational database management system.

This application supports GKE On Prem deployment.

Learn more

# About Google Click to Deploy containers

Type

Kubernetes apps

Last updated

11/23/20

In this case, we decide to change the default parameters and require a higher number of replicas=2 (see Fig. 14).

Figure 17.14: Deploying a MariaDB solution with a higher number of replicas.

![](img/fb3bebce7b5f593bca1a7f8a1371be06_648_0.png)

![](img/fb3bebce7b5f593bca1a7f8a1371be06_648_1.png)

The result of our deployment is pretty straightforward (see Fig. 15). Once again, Marketplace allows us to save time while administering our Anthos clusters.

## Figure 17.15: MariaDB deployed solution

Marketplace
Deploy MariaDB
us-central1-a
Create cluster
or Select an existing cluster
Namespace
default
App instance name
mariadb-1
StorageClass
Create a new storage class
Storage size for persistent volumes
32Gi
Replicas
2

|  | mariadb-1-mariadb | OK | Stateful Set | 1/1 | default | cluster-5 |
|  | mariadb-1-mariadb-secondary | OK | Stateful Set | 2/2 | default | cluster-5 |

Once MariaDB is deployed from the marketplace, you can use Anthos to manage the cluster. In figure 16, you see an example of inspecting the cluster and the running pods.

## Figure 17.16: MariaDB inspecting the solution deployed from the marketplace

| Field | Value |
|---|---|
| Cluster | cluster-5 |
| Namespace | default |
| Created | Nov 25, 2020, 2:51:23 PM |
| Labels | app.kubernetes.io/component: mariadb-server; app.kubernetes.io/name: mariadb-1 |
| Annotations | SHOW ANNOTATIONS |
| Logs | Container logs, Audit logs |
| Label selector | app.kubernetes.io/component = mariadb-server; app.kubernetes.io/name = mariadb-1 |
| Replicas | 1 |
| Observed generations | 1 |
| Service | mariadb-1-mariadb |
| Persistent volume claims specification | mariadb-1-data-pvc |

# Pod specification

| Field | Value |
|---|---|
| Labels | app.kubernetes.io/component: mariadb-server; app.kubernetes.io/name: mariadb-1 |
| Termination grace period | 30 |
| Restart policy | Always |
| Containers | mariadb, mysqld-exporter |
| Volumes | configmap, tls-volume, replication-config |

# Managed pods

| Name | Status | Restarts | Created on |
|---|---|---:|---|
| mariadb-1-mariadb-0 | Running | 0 | Nov 25, 2020, 2:51:23 PM |

## 17.2.3 What we have done so far

So far we have installed two different applications on Anthos cluster on GKE. If we want to check the status we can simply access our GKE under 'Applications' (see Fig.16).

https://console.cloud.google.com/kubernetes/application

![](img/fb3bebce7b5f593bca1a7f8a1371be06_652_0.png)

## 17.2.4 Example 3: Cassandra

Cassandra is a NoSQL, highly scalable, high-performance distributed database with high availability. Deploying a Cassandra cluster is easy. You can access the solution at:

https://console.cloud.google.com/marketplace/details/google/cassandra?q=anthos

![](img/fb3bebce7b5f593bca1a7f8a1371be06_652_1.png)

![](img/fb3bebce7b5f593bca1a7f8a1371be06_653_0.png)

# Cassandra Cluster

Container Registry tag: Cassandra 3.11.x

Cassandra Cluster (Google Click to Deploy containers)

NoSQL database with masterless replication for high availability

CONFIGURE

![](img/fb3bebce7b5f593bca1a7f8a1371be06_653_1.png)

# Overview

# Type

Kubernetes apps

# Last updated

11/23/20

Cassandra is a NoSQL, highly scalable, high-performance distributed database with high availability with no single point of failure.

This application supports GKE On Prem deployment.

This application provides Prometheus metrics, and supports Stackdriver integration.

Learn more

After installing (see Fig. 17), you can access and manage your deployed solutions. For instance you might be interested in monitoring CPU, Memory and disk usage for a given period of time (see Fig. 18).

## Figure 17.19: Managing a Cassandra solution

![](img/fb3bebce7b5f593bca1a7f8a1371be06_654_0.png)

### 17.2.5 Example 4: Prometheus & Grafana

Once you understand the mechanism, installing a new solution is streamlined. For instance, suppose that we want to install a more complex set of applications that work together. A classical example is Prometheus and Grafana. Prometheus is an open-source monitoring and alerting platform adopted by many companies as a monitoring tool, and Grafana provides a number of dashboards which visualize the metrics collected by the Prometheus server. The solution can be accessed at (see Fig. 19):

https://console.cloud.google.com/marketplace/details/google/prometheus?q=anthos

**Figure 17.20: Deploying a Prometheus and Grafana solution**

![](img/fb3bebce7b5f593bca1a7f8a1371be06_655_0.png)

# Overview

Prometheus is an open-source monitoring and alerting platform, widely adopted by many companies as a Kubernetes monitoring tool. In this application Prometheus is supported by Grafana, a highly customizable user interface providing a number of preinstalled dashboards visualizing the metrics collected by the Prometheus server.

This application supports GKE On Prem deployment.

Let’s use the default parameters for deploying the solution (see Fig. 20):

## Figure 17.21: Using the default parameters for the Prometheus and Grafana solution

# Marketplace

# Deploy

Click to Deploy on GKE

Deploy via command line

Cluster

cluster-1 [us-central1-a]

or Create a new cluster

Namespace

default

App instance name

prometheus-1

Prometheus replicas

2

Prometheus Service Account

Create a new service account

Kube State Metrics Service Account

Create a new service account

Alertmanager Service Account

Create a new service account

Grafana Service Account

Create a new service account

Once deployed we can see the solution available from the Application details (see Fig.21):

Figure 17.22: Prometheus and Grafana deployed solution
