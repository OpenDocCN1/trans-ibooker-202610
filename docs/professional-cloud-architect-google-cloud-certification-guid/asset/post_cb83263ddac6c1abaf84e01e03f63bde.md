
Cloud SQL for MySQL offers us the following:

- MySQL currently offers support for MySQL 5.6, 5.7, and 8.0 and can provide up to a maximum of 624 GB of RAM and 64 TB of storage when selecting a high memory instance type.
- There's data replication between multiple zones with automatic failover.
- Fully managed MySQL Community Edition databases are available in the cloud.
- There's point-in-time recovery and on-demand backups.
- There's instance cloning.
- It offers integration with GCP's monitoring and logging solutions – Cloud Operations.

Cloud SQL for PostgreSQL offers us the following:

- A fully managed PostgreSQL database in the cloud with up to 64 TB of storage, depending on whether the instance has dedicated or shared vCPUs
- Custom machine types with up to 624 GB RAM and 96 CPUs
- Data replication between multiple zones with automatic failover
- Instance cloning
- On-demand backups
- Integration with GCP's monitoring and logging solutions – Cloud Operations logging and monitoring

Cloud SQL for SQL Server offers us the following:

- A fully-managed SQL Server database in the cloud.
- Custom machine types with up to 624 GB RAM and 96 CPUs.
- Up to 64 TB of storage.
- Custom data encrypted in Google's internal networks and database tables, temporary files, and backups.
- Databases can be imported using BAK and SQL files.
- Instance cloning.
- Integration with Cloud Operations logging and monitoring.
- Data replication between multiple regions.
- HA through regional persistent disks.

Now that we know the main features, let's look at some of the things to bear in mind when we create a new instance:

- **Selecting an instance**: Similar to other GCP services, we have the option to select a location for our instances. It makes sense to place our database instance close to the services that depend on it. We should understand our database requirements and have a baseline of active connections, memory, and CPU usage to allow us to select the correct machine type. If we over-spec, this will have an impact on our cost, and likewise, if we under-spec, we may impact our performance or availability if resources are exhausted.
- **Selecting region**: We have the option to select the region that our instance will be deployed to. Be mindful that the region cannot be changed after deployment, but the zone can be changed at any time. For HA, we can select multiple zones, which offers automatic failover to another zone in our selected region. From a performance standpoint, we should be looking to deploy our instances close to the services that are using them. We will look at HA in more detail later in this chapter.
- **Selecting storage**: When creating an instance, we can select different storage tiers. Depending on our requirements, we may wish to select an SSD for lower latency and higher throughput; however, we can also select an HDD if we don't require such a high-performing disk.
- **Selecting encryption**: When creating an instance, we can choose to have Google manage our encryption keys, which is the default, or select a customer-managed key, which is managed via the Google KMS service.

Selecting the correct capacity to fit your database size is also extremely important because once you have created your instance, you cannot decrease the capacity. If we over-spec our storage, then we will be paying for unused space. If we under-spec our storage, then we can cause issues with availability. One method to avoid this is through the automatic storage increase setting. If this is enabled, your storage is checked every 30 seconds to ensure that the storage has not fallen below a set threshold size. If it has, then additional storage will be automatically added to your instance. The threshold's size depends on the amount of storage your instance currently has provisioned, and it cannot be larger than 25 GB. This sounds great, but we should also be mindful that if we have spikes in demand, then we will suffer a permanent increase in storage cost. We should also be mindful that creating or increasing storage capacity to 30 TB or greater could lead to increased latency for operations such as backups:

- **Selecting an IP address:** We can select from a public or a private IP address. The default is to use a public IP address, which will block all IP addresses. So, we must add specific IP addresses or ranges to open our instances up. To use a private IP address, we must enable Google's Service Networking API. This is required for each project. If we wish to use a private IP address, we need to ensure that we have VPC peering set up between the Google service VPC network where our instance resides and our VPC network. This is shown in the following diagram:

![](img/de2c97ab017f36b3750a5bade111edd7_354_0.png)

Figure 11.24 – VPC peering (Source: https://cloud.google.com/sql/docs/postgres/private-ip. License: https://creativecommons.org/licenses/by/4.0/legalcode)

We discussed VPC peering in more detail in Chapter 10, Networking Options in GCP. Using a private IP will help lower network latency and improve security as traffic will never be exposed to the internet; however, we should also note that to access a Cloud SQL instance on its private IP address from other GCP resources, the resource must be in the same region.

> **Important Note**
> It is recommended that you use a private IP address if you wish to access a Cloud SQL instance from an application that is running in Google's container service; that is, Google Kubernetes Engine.

- **Automatic backups**: Enabling automatic backups will have a small impact on performance; however, we should be willing to accept this to take advantage of features such as read replicas, cloning, or point-in-time recovery. We can select where our backups are located. However, by default, backups are stored in the closest multi-region location, so they should only be customized based on unique design decisions. Additionally, we can select how many automatic backups are stored. This defaults to 7 and again, unless there is a specific reason to change this, then it is not recommended to modify it. Also, please note that if multiple zones are selected, then automatic backups are enabled by default and cannot be disabled.
- **Maintenance windows**: When working with this offering, we should also specify a maintenance window. We can choose a specific day and 1-hour time slot when updates can be applied. GCP will not initiate any update to that instance within this specified window. Please note that if no window is specified, then potentially disruptive updates can occur at any time.

## Data consistency

Cloud SQL is a relational database that offers strong consistency and support for ACID transactions.

## Creating and managing Cloud SQL

Now that we have an understanding of the concepts, let's look at how to create a new MySQL instance:

- 1. Go to the navigation menu and go to **DATABASES | SQL:**
- 2. Click on **Create an instance**.
- 3. Select the database engine you require. In our example, we will select **MySQL**:

![](img/de2c97ab017f36b3750a5bade111edd7_356_0.png)

Figure 11.25 – Selecting SQL

Choose your database engine

![](img/de2c97ab017f36b3750a5bade111edd7_356_1.png)

Figure 11.26 – Choose your database engine

4. Give the instance a name and a password. You can either enter your own password or click Generate to receive a random password. Additionally, you can select a location and database version. We will leave the defaults in this example as-is:

## Create a MySQL instance

## Instance info

Instance ID *
cloudarchitect

Use lowercase letters, numbers, and hyphens. Start with a letter.

Password *
••••••••••••••
GENERATE
Set a password for the root user. Learn more

No password

Database version *
MySQL 8.0

## Choose region and zonal availability

For better performance, keep your data close to the services that need it. Region is permanent, while zone can be changed any time.

Region
europe-west2 (London)

Single zone
In case of outage, no failover. Not recommended for production.

Multiple zones (Highly available)

Figure 11.27 – Create a MySQL instance
