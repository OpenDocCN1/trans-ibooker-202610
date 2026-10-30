
- **Regional:** This will result in a 99.99% SLA, which is still very high and equivalent to approximately only 52 minutes of downtime per year. Regional instances should be selected if users and services are within the same region. This will offer the lowest latency. As you can see, the requirements will dictate the best option. We cannot change the instance's configuration after creation. Regional configurations will contain three read/write replicas that allow us to meet any governance requirements regarding where our data is located.

## Node count

We are also required to select the number of nodes to allocate to our instance. This will determine the amount of CPU/RAM and storage resources that are available to our instance. Each node will provide up to 2 TB of storage, and it is recommended that a minimum of three nodes be used for production environments. We can change the number of nodes after creation to scale up or down.

## Processing units

Cloud Spanner now supports processing units. 1,000 processing units is equivalent to 1 node and each increment of 100 units will behave like 1/10th of a node.

## Replication

We mentioned replicas previously, and it's important to take a closer look at replication within Cloud Spanner. The underlying distributed filesystem that Cloud Spanner is built on will automatically replicate at the byte level. However, to provide additional data availability and geographic locality, Cloud Spanner will also replicate data by creating copies (replicas) of the rows that Cloud Spanner organized data into. These copies are then stored in a different geographic area. One of these replicas is elected to act as the leader and will be responsible for handling writes. Cloud Spanner has three types of replicas:

- **Read/Write:** This type of replication will maintain a full copy of our data and is eligible to become a leader. This is the only type of replication that's available to single-region instances.
- **Read-Only:** This type of replication will only support reads and cannot become a leader. It will maintain a full copy of our data, which has been replicated from our read/write replica. Read-Only is only available in multi-regional configurations.
- **Witness:** This type of replication doesn't support reads, nor does it maintain a full copy of our data. Witness replication makes it easier for us to achieve quorums for writes without the compute resources that are required by a read/write replica. Witness is only available in multi-regional configurations.

## TrueTime

TrueTime is a globally distributed clock built on GPS and atomic clocks. It returns a time interval that is guaranteed to contain the clock's actual time. TrueTime is provided to applications on Google Servers, which allows applications to generate monotonically increasing timestamps. Cloud Spanner uses TrueTime to assign timestamps to transactions whenever Cloud Spanner deems the transaction to have taken place.

## Data consistency

If you are looking for guaranteed strong consistency, then Cloud Spanner is the database offering for you! This is all down to the fact it is designed for the cloud and, therefore, will negate a lot of the constraints of other database offerings. Note that Cloud Spanner provides external consistency, which is a stronger property than strong consistency. This is the strictest consistency property for transaction-processing systems.

When we speak about data consistency concerning Cloud Spanner, it is wise to raise awareness of the concept of the CAP theorem. This theorem says that a database can only have two out of three of the following properties:

- **C: Consistency**. This implies a single value for shared data.
- **A: Availability**. 100% for both reads and updates.
- **P: Partitions**. Tolerance for network partitions.

This means that systems can be made up of CA, CP, or AP. The theorem is about 100% availability, but we know that systems will not offer 100% availability; therefore, system developers are forced to think seriously about the trade-off. If we believe that some network partitions are inevitable at some point in a system's life cycle, then a distributed system should be designed to forfeit consistency or availability – but only during a partition. Cloud Spanner, in times of a partition, will forfeit availability to ensure consistency. Given that Spanner offers 99.999% availability, which equates to less than 5.26 minutes of downtime per year, this is deemed to be high enough that we don't need to worry about availability trade-offs.

## Encryption

By default, all data in Cloud Spanner will use Google-managed default encryption. When **Customer-Managed Encryption Keys (CMEK)** are enabled, Cloud Spanner will use our Cloud **Key Management Service (KMS)** keys to protect data at rest. To use CMEK, we must specify the Cloud KMS key when the database is created. As of June 2021, Cloud Spanner supports **Cloud External Key Manager (Cloud EKM)** when using CMEK.

## Point-in-time recovery

Cloud Spanner offers protection against accidental write or deletions. This is known as **Point-in-Time Recovery (PITR)**. If, for example, an application rollout corrupts a database, then PITR can recover your data from a point in time from the past 7 days. PITR works by allowing us to configure the database's `version_retention_period` to retain all the versions of the data and its schema. Retention periods can range from 1 hour to 7 days. We can restore either the entire database or a portion of it, but we should also consider performance when we set longer retention periods as this will use more system resources, particularly on those databases that frequently overwrite data.

Next, let's look at how we can create our first Spanner instance.

## Creating a Cloud Spanner instance

Let's look at actually creating a Cloud Spanner instance. We will also create a database – that is, a table – and run a query on it:

1. Browse to **DATABASES | Spanner**, as shown in the following screenshot:

![](img/de2c97ab017f36b3750a5bade111edd7_375_0.png)

Figure 12.1 – Browsing to Cloud Spanner

2. Choose to create an instance, and then enter an instance name. Please note that you may have to enable the API. ID will be auto-populated based on our name, but this can be modified if needed. We can also select our regional configuration and our compute capacity. Click on Create to provision the instance:

# Name your instance

An instance has both a name and an ID. The name is for display purposes only. The ID is a permanent and unique identifier.

Instance name *
cloudarchitect
Name must be 4-30 characters long

Instance ID *
cloudarchitect
Lowercase letters, numbers, hyphens allowed

# Choose a configuration

Determines where your nodes and data are located. Affects cost, performance, and replication. A multi-region configuration will select the default leader region for your leader replicas. You can change your leader region at any time with a DDL statement. Learn more

COMPARE REGION CONFIGURATIONS

Regional
Multi-region
 eur3

# Allocate compute capacity

Your compute capacity determines the amount of data throughput, queries per second (QPS), and storage limits in your instance. One node equals 1,000 processing units. Affects billing.

Unit *
Processing units
Quantity *
1000
Integers only. Enter in increments of 100 up to 1,000, followed by increments of 1,000.

Figure 12.2 – Creating a new instance

3. Once our instance has been created, click on Create database:

# Create a database in cloudarchitect

# Name your database

Enter a permanent name for your database of at least two characters, starting with a letter.

Database name *

exam_tracker

Lowercase letters, numbers, hyphens, underscores allowed

Figure 12.3 – Creating a database

4. Now, we can create a table. Let’s create a table called professionalCloudArchitect. Our table will keep track of how employees did while sitting the Google Professional Cloud Architect exam. Once you have populated the text field as follows, click SUBMIT to create the table:

# Define exam_tracker schema

Use Cloud Spanner’s Data Definition Language (DDL) to define the schema in your instance. DDL statements let you alter a database; create, alter, or drop tables in a database; and create or drop indexes in a database. To write multiple statements, separate them with a semicolon. Learn more

DDL TEMPLATES  SHORTCUTS

```
CREATE TABLE professionalCloudArchitect (
EmployeeId INT64 NOT NULL,
NAME STRING(1024),
Result STRING(1024),
) PRIMARY KEY (EmployeeId);
```

Figure 12.4 – Creating a table
