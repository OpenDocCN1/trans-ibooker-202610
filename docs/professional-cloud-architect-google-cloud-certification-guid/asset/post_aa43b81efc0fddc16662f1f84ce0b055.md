
## Understanding IAM

Access to Google Cloud Storage is secured with IAM. Let's have a look at the following list of predefined roles and their details:

- **Storage Object Creator**: Has rights to create objects but does not give permissions to view, delete, or overwrite objects
- **Storage Object Viewer**: Has rights to view objects and their metadata, but not the ACL, and has rights to list the objects in a bucket
- **Storage Object Admin**: Has full control over objects and can create, view, and delete objects
- **Storage Admin**: Has full control over buckets and objects
- **Storage HMAC Key Admin**: Has full control over HMAC keys in a project and can only be applied to a project

## Quotas and limits

Google Cloud Storage comes with predefined quotas. These default quotas can be changed via the **Navigation** menu, under the **IAM & Admin | Quotas** section. From this menu, we can review the current quotas and request an increase to these limits. We recommend that you familiarize yourself with the limits of each service as this can have an impact on your scalability. For Cloud Storage, we should be aware of the following limits:

- Individual objects are limited to a maximum size of 5 TiB.
- Updates to an individual object are limited to one per second.
- There is an initial limit of 1,000 writes per second per bucket.
- There is an initial limit of 5,000 reads per second per bucket.
- There is a limit of 100 ACL entries per object.
- There is a limit of one bucket creation operation every 2 seconds.
- There is a limit of one bucket deletion operation every 2 seconds.

## Pricing

Pricing for Google Cloud Storage is based on four components:

- **Data storage**: This applies to at-rest data that is stored in Cloud Storage, is charged per GB per month, and the cost depends on the location and class of the storage.
- **Network usage**: This applies when object data or metadata is read from or moved between our buckets.
- **Operations usage**: This applies when you perform actions within Cloud Storage that make changes or retrieve information about buckets and their objects. The cost depends on the class of the storage.
- **Retrieval and early deletion fees**: This applies when accessing data from the Nearline, Coldline, and Archive storage classes. Retrieval costs will apply when we read, copy, or rewrite data or metadata. The minimum storage duration applies to data stored in Nearline, Coldline, and Archive storage. You will still be charged for the duration of an object, even if the file is deleted before the minimal period.

## Understanding Cloud Firestore

It's wise to note early on in this section that Google rebranded their previous Cloud Datastore product with a newer major version called **Cloud Firestore**. However, it is also important to understand the relationship between both as Firestore gives the option of Native mode and Datastore mode, with the latter using the traditional Datastore system's behaviors with the Firestore storage layer, which eliminates some of Datastore's limitations.

Cloud Datastore is a NoSQL database that is built to ease application development. It uses a distributed architecture that is highly scalable, and because it is serverless, we do not have to worry about the underlying infrastructure. Cloud Datastore distributes our data over several machines and uses masterless, synchronous replication over a wide geographical area.

Firestore builds on this but also accelerates the development of mobile, IoT, and web applications. It offers built-in live synchronization, as well as an offline mode that increases the efficiency of developing real-time applications. This includes workloads consisting of live asset tracking, real-time analytics, social media profiles, and gaming leaderboards.

Firestore also offers very **high availability (HA)** on our data. With automatic multi-region replication and strong consistency, it offers a 99.999% guarantee.

Before we go any further into the details, we don't want to make any assumptions that you know what a NoSQL database is. SQL databases are far more well-known by non-database administrators in the industry. A SQL database is primarily a relational database that is based on tables consisting of several rows of data that have predefined schemas.

Compare that to the concept of a NoSQL database, which is a non-relational database based around key-value pairs that do not have to adhere to any schema definitions. Ultimately, this allows us to scale efficiently; therefore, we can see how it becomes a lot easier to add or remove resources as we need to. This elasticity makes Cloud Datastore perfect for transactional information, such as real-time inventories, ACID transactions that will always provide data that is valid, or for providing user profiles to keep a record of user experience based on past activities or preferences. However, it is also important to note that NoSQL databases may not be the perfect fit for every scenario and consideration should be taken when deciding on what database to select, particularly regarding the efficiency of querying your data. OK; so, now that we know what a NoSQL database is, let’s look at what makes up a Datastore database, as relationships between data objects are addressed differently than other databases that you may be familiar with. A traditional SQL database would be made up of tables, rows, and columns. In Cloud Datastore, an entity is the equivalent of a row, and a kind is the equivalent of a table. Entities are data objects that can have one or more named properties.

Each entity has a key that identifies it. Let’s look at the following table to compare the terminology that is used with SQL databases and Cloud Datastore:

| Concept | SQL Database | Cloud Datastore |
|---|---|---|
| Category of object | Table | Kind |
| One object | Row | Entity |
| Individual data for an object | Column | Property |
| Unique ID for an object | Primary Key | Key |

Figure 11.14 – Terminology comparison

Now, let’s look at the different types of modes that are offered in more detail.

## Cloud Firestore in Datastore mode or Native mode

When we create a new database, we must select one of the modes. However, we cannot use a mixture of modes within the same project. So, it is recommended to think about your design and what your application is built for.

Native mode offers all the new features of the revamped product. It takes the best of Cloud Datastore and the Firebase real-time database. Firebase was originally designed for mobile applications that required synced states across clients in real time. As such, Firestore takes advantage of this and offers a new, strongly consistent storage layer, as well as real-time updates. Additionally, it offers a collection and document data model and mobile and web client libraries. Native mode can scale automatically to millions of concurrent clients.

> **Exam Tip**
> While Firestore is backward compatible with Datastore, the new data model, real-time updates, and mobile and web client libraries are not. You must use Native mode to gain access to these features.

Datastore mode supports the Datastore API, so there is no requirement to change any existing Datastore applications. If your architecture requires the use of an established Datastore server architecture, then Datastore mode allows for this and removes some of the limitations from Datastore, mainly that Datastore mode offers strong consistency unless eventual consistency is specified. Datastore mode can scale automatically to millions of writes per second.

> **Exam Tip**
> Data objects in Firestore in Datastore mode are known as entities.

## Upgrading to Firestore

New projects that require a Datastore database should use Firestore in Datastore mode. However, starting in 2021, existing Datastore databases will be automatically upgraded without the need to update your application code. Google will notify users of the schedule for the upgrade, which does not require downtime.

## Creating and using Cloud Datastore

The first thing we need to do is select a mode before we can utilize Cloud Firestore. Let’s look at creating a database in Datastore mode and run some queries on the entities that we create. Before we do so, let’s remind ourselves that Datastore is a managed service. As we go through this process, you will note that we are directly creating our entities without provisioning any underlying infrastructure first. You should also note that, like all storage services from GCP, Cloud Datastore automatically encrypts all data before it’s written to disk. Follow these steps to create a Firestore in Datastore mode:

1. From our GCP Console, browse to **DATABASES Firestore:**

![](img/de2c97ab017f36b3750a5bade111edd7_344_0.png)
