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

# Cloud Firestore in Datastore mode or Native mode

When we create a new database, we must select one of the modes. However, we cannot use a mixture of modes within the same project. So, it is recommended to think about your design and what your application is built for.

Native mode offers all the new features of the revamped product. It takes the best of Cloud Datastore and the Firebase real-time database. Firebase was originally designed for mobile applications that required synced states across clients in real time. As such, Firestore takes advantage of this and offers a new, strongly consistent storage layer, as well as real-time updates. Additionally, it offers a collection and document data model and mobile and web client libraries. Native mode can scale automatically to millions of concurrent clients.