# Pricing

Pricing for Google Cloud Storage is based on four components:

+   - **Data storage**: This applies to at-rest data that is stored in Cloud Storage, is charged per GB per month, and the cost depends on the location and class of the storage.
+   - **Network usage**: This applies when object data or metadata is read from or moved between our buckets.
+   - **Operations usage**: This applies when you perform actions within Cloud Storage that make changes or retrieve information about buckets and their objects. The cost depends on the class of the storage.
+   - **Retrieval and early deletion fees**: This applies when accessing data from the Nearline, Coldline, and Archive storage classes. Retrieval costs will apply when we read, copy, or rewrite data or metadata. The minimum storage duration applies to data stored in Nearline, Coldline, and Archive storage. You will still be charged for the duration of an object, even if the file is deleted before the minimal period.

# Understanding Cloud Firestore

It's wise to note early on in this section that Google rebranded their previous Cloud Datastore product with a newer major version called **Cloud Firestore**. However, it is also important to understand the relationship between both as Firestore gives the option of Native mode and Datastore mode, with the latter using the traditional Datastore system's behaviors with the Firestore storage layer, which eliminates some of Datastore's limitations.

Cloud Datastore is a NoSQL database that is built to ease application development. It uses a distributed architecture that is highly scalable, and because it is serverless, we do not have to worry about the underlying infrastructure. Cloud Datastore distributes our data over several machines and uses masterless, synchronous replication over a wide geographical area.

Firestore builds on this but also accelerates the development of mobile, IoT, and web applications. It offers built-in live synchronization, as well as an offline mode that increases the efficiency of developing real-time applications. This includes workloads consisting of live asset tracking, real-time analytics, social media profiles, and gaming leaderboards.

Firestore also offers very **high availability (HA)** on our data. With automatic multi-region replication and strong consistency, it offers a 99.999% guarantee.

Before we go any further into the details, we don't want to make any assumptions that you know what a NoSQL database is. SQL databases are far more well-known by non-database administrators in the industry. A SQL database is primarily a relational database that is based on tables consisting of several rows of data that have predefined schemas.