
![](img/1711f142ae3cfd740ef106afbbba2046_362_0.png)

## Quick Summary

- Applications that process large amounts of data from a query should consider changing the fetch size of the data.
- A trade-off exists between loading too much data in the application (putting pressure on the garbage collector) and making frequent database calls to retrieve a set of data.

## JPA

The performance of JPA is directly affected by the performance of the underlying JDBC driver, and most of the performance considerations regarding the JDBC driver apply to JPA. JPA has additional performance considerations.

JPA achieves many of its performance enhancements by altering the bytecode of the entity classes. In most server frameworks, this happens transparently. In a Java SE environment, it is important to make sure that the bytecode processing is set up correctly. Otherwise, JPA application performance will be unpredictable: fields that are expected to be loaded lazily might be loaded eagerly, data saved to the database might be redundant, data that should be in the JPA cache may need to be refetched from the database, and so on.

There is no JPA-defined way for the bytecode to be processed. Typically, this is done as part of compilation—after the entity classes are compiled (and before they are loaded into JAR files or run by the JVM), they are passed through an implementation-specific postprocessor that “enhances” the bytecode, producing an altered class file with the desired optimizations. Hibernate, for example, does this via a Maven or Gradle plug-in during compilation.

Some JPA implementations also provide a way to dynamically enhance the bytecode as the classes are loaded into the JVM. This requires running an agent within the JVM that is notified when classes are loaded; the agent interposes on the classloading and alters the bytes before they are used to define the class. The agent is specified on the command line of the application; for example, for EclipseLink you include the `-javaagent:path_to/eclipselink.jar` argument.

## Optimizing JPA Writes

In JDBC, we looked at two critical performance techniques: reusing prepared statements and performing updates in batches. It is possible to accomplish both of those optimizations with JPA, but the way it is done depends on the JPA implementation in use; there are no calls within the JPA API to do that. For Java SE, these optimizations typically require setting a particular property in the application’s *persistence.xml* file.

## Writing Fewer Fields

One common way to optimize writes to a database is to write only those fields that have changed. The code the HR system uses to double my salary may need to retrieve 20 fields from my employee record, but only one (very important) field needs to be written back to the database.

JPA implementations should be expected to perform this optimization transparently. This is one of the reasons JPA bytecode must be enhanced, since that is the process by which the JPA provider keeps track of when values in the code are changed. When the JPA code is properly enhanced, the SQL to write my doubled salary back to the database will update only that single column.

For example, using the JPA EclipseLink reference implementation, statement reuse is enabled by adding the following property to the persistence.xml file:

```
<property name="eclipselink.jdbc.cache-statements" value="true" />
```

Note that this enables statement reuse within the EclipseLink implementation. If the JDBC driver is capable of providing a statement pool, it is usually preferable to enable the statement caching in the driver and to leave this property out of the JPA configuration.

Statement batching in the reference JPA implementation is achieved by adding these properties:

```
<property name="eclipselink.jdbc.batch-writing" value="JDBC" />
<property name="eclipselink.jdbc.batch-writing.size" value="10000" />
```

JDBC drivers cannot automatically implement statement batching, so this is a useful property to set in all cases. The batch size can be controlled in two ways: first, the size property can be set, as is done in this example. Second, the application can periodically call the flush() method of the entity manager, which will cause all batched statements to be executed immediately.

Table 11-2 shows the effect of the statement reuse and batching to create and write stock entities into the database.

## Table 11-2. Seconds required to insert data for 256 stocks via JPA

| Programming mode | Time required |
|---|---|
| No batching, no statement pool | 83 ± 3 seconds |
| No batching, statement pool | 64 ± 5 seconds |
| Batching, no statement pool | 10 ± 0.4 seconds |
| Batching, statement pooling | 10 ± 0.3 seconds |

![](img/1711f142ae3cfd740ef106afbbba2046_364_0.png)

## Quick Summary

- JPA applications, like JDBC applications, can benefit from limiting the number of write calls to the database (with the potential trade-off of holding transaction locks).
- Statement caching can be achieved at the JPA layer or the JDBC layer. Caching at the JDBC layer should be explored first.
- Batching JPA updates can be done declaratively (in the persistence.xml file) or programmatically (by calling the flush() method).

## Optimizing JPA Reads

Optimizing when and how JPA reads data from the database is more complicated than it might seem, because JPA will cache data in the hope that it might be used to satisfy a future request. That’s usually a good thing for performance, but it means that the JPA-generated SQL used to read that data may seem, on the face of it, suboptimal. The data retrieval is optimized to serve the needs of the JPA cache, rather than being optimized for whatever particular request is in progress.

The details of the cache are covered in the next section. For now, let’s look at the basic ways to apply database read optimizations to JPA. JPA reads data from the database in three cases: when the find() method of the EntityManager is called, when a JPA query is executed, and when code navigates to a new entity using the relationship of an existing entity. In the stock class, that latter case means calling the getOptions() method on a Stock entity.

Calling the find() method is the most straightforward case: only a single row is involved, and (at least) that single row is read from the database. The only thing that can be controlled is the amount of data retrieved. JPA can retrieve only some of the fields in the row, it can retrieve the entire row, or it can prefetch other entities related to the row being retrieved. Those optimizations apply to queries as well.

Two possible paths are available: read less data (because the data won’t be needed) or read more data at a time (because that data will definitely be needed in the future).

## Reading less data

To read less data, specify that the field in question is loaded lazily. When an entity is retrieved, the fields with a lazy annotation will be excluded from the SQL used to load the data. If the getter of that field is ever executed, it will mean another trip to the database to retrieve that piece of data.

here: a common expectation about eager relationship fetching is that it will employ a JOIN in the generated SQL. In typical JPA providers, that is not the case: they will issue a single SQL query to fetch the primary object, and then one or more SQL commands to fetch any additional, related objects. From a simple find() method, there is no control over this: if a JOIN statement is required, you will have to use a query and program the JOIN into the query.

## Using JOIN in queries

The JPA Query Language (JPQL) doesn’t allow you to specify fields of an object to be retrieved. Take the following JPQL query:

```
Query q = em.createQuery("SELECT s FROM StockPriceImpl s");
```

That query will always yield this SQL statement:

```
SELECT <enumerated list of non-LAZY fields> FROM StockPriceTable
```

If you want to retrieve fewer fields in the generated SQL, you have no option but to mark them as lazy. Similarly, for fields that are marked as lazy, there is no real option for fetching them in a query.

If relationships exist between entities, the entities can be explicitly joined in a query in JPQL, which will retrieve the initial entities and their related entities in one shot. For example, in the stock entities, this query can be issued:

```
Query q = em.createQuery("SELECT s FROM StockOptionImpl s " +
"JOIN FETCH s.optionsPrices");
```

That results in an SQL statement similar to this:

```
SELECT t1.<fields>, t0.<fields> FROM StockOptionPrice t0, StockPrice t1
WHERE ((t0.SYMBOL = t1.SYMBOL) AND (t0.PRICEDATE = t1.PRICEDATE))
```

## Other Mechanisms for a Join Fetch

Many JPA providers allow a join fetch to be specified by setting query hints on a query. For example, in EclipseLink, this code will produce a JOIN query:

```
Query q = em.createQuery("SELECT s FROM StockOptionImpl s");
q.setQueryHint("eclipselink.join-fetch", "s.optionsPrices");
```

Some JPA providers also have a special @JoinFetch annotation that can be used on the relationship.

The exact SQL will differ among JPA providers (this example is from EclipseLink), but this is the general process.
