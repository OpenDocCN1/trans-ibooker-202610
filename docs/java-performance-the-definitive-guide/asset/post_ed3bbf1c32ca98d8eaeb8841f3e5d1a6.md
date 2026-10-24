
## Join fetch and caching

As discussed in the previous section, the query could be written to explicitly use a JOIN statement:

```java
@NamedQuery(name="findAll",
    query="SELECT s FROM StockPriceEagerLazyImpl s " +
          "JOIN FETCH s.options ORDER BY s.id.symbol")
```

Using that named query (with full traversal) yields the data in **Table 11-5**.

*Table 11-5. Seconds required to read data for 256 stocks (JOIN query)*

| Test case | First execution | Subsequent executions |
|---|---|---|
| Default configuration | 22.7 ± 2 seconds (66,817 SQL calls) | 1.1 ± 0.7 seconds (1 SQL call) |
| Join fetch | 9.0 ± 0.3 seconds (1 SQL call) | 5.6 ± 0.4 seconds (1 SQL call) |
| Join fetch with query cache | 5.8 ± 0.2 seconds (1 SQL call) | 0.001 ± 0.0001 seconds (0 SQL calls) |

The first time the loop is executed with a JOIN query, a big performance win results: it takes only 9 seconds. That is the result of issuing only one SQL request rather than 66,817 of them.

Unfortunately, the next time the code is executed, it still needs that one SQL statement, since query results are not in the L2 cache. Subsequent executions of the example take 5.6 seconds—because the SQL statement that is executed has the JOIN statement and is retrieving more than 400,000 rows of data.

If the JPA provider implements query caching, this is clearly a good time to use it. If no SQL statements are required during the second execution of the code, only 1 ms is required on the subsequent executions. Be aware that query caching works only if the parameters used in the query are exactly the same each time the query is executed.

## Avoiding queries

If entities are never retrieved via a query, all entities can be accessed through the L2 cache after an initial warm-up period. The L2 cache can be warmed up by loading all entities, so slightly modifying the previous example gives this code:

```java
EntityManager em = emf.createEntityManager();
ArrayList<String> allSymbols = ... all valid symbols ...;
ArrayList<Date> allDates = ... all valid dates...;
for (String symbol : allSymbols) {
    for (Date date : allDates) {
        StockPrice sp =
            em.find(StockPriceImpl.class, new StockPricePK(symbol, date));
        ... process sp ...
        if (processOptions) {
            Collection<? extends StockOptionPrice> options = sp.getOptions();
            ... process options ...
        }
    }
}
```

The results of executing this code are given in Table 11-6.

*Table 11-6. Seconds required to read data for 256 stocks (L2 cache used)*

| Test case | First execution | Subsequent executions |
|---|---|---|
| Default configuration | 22.7 ± 2 seconds (66,817 SQL calls) | 1.1 ± 0.7 seconds (1 SQL call) |
| No query | 35 ± 3 seconds (133,632 SQL calls) | 0.28 ± 0.3 seconds (0 SQL calls) |

The first execution of this loop requires 133,632 SQL statements: 66,816 for the call to the find() method, and an additional 66,816 for the call to the getOptions() method. Subsequent executions of that code are very fast indeed, since all the entities are in the L2 cache, and no SQL statements need to be issued.

## Warming Up a Test

Java performance tests—and particularly benchmarks—usually have a warm-up period. As discussed in Chapter 4, that allows the compiler to compile the code optimally.

Here’s another example where a warm-up period is beneficial. During the warm-up period of a JPA application, the most-frequently used entities will be loaded into the L2 cache. The measurement period of the test will see very different performance as those entities are first loaded. This is particularly true if, as in the previous example, no queries are used to load entities, but that is generally the difference in all the tables in this section between first and second executions.

Recall that the sample database includes five option prices for every date and symbol pair, or a total of 334,080 option prices for 256 stocks over one year of data. When the five stock options for a particular symbol and date are accessed via a relationship, they can all be retrieved at once. That’s why only 66,816 SQL statements are required to load all the option price data. Even though multiple rows are returned from those SQL statements, JPA is still able to cache the entities—it is not the same thing as executing a query. If the L2 cache is warmed up by iterating through entities, don’t iterate through related entities individually—do that by simply visiting the relationship.

As code is optimized, you must take into account the effects of the cache (and particularly the L2 cache). Even if you think you could write better SQL than what JPA generates (and hence should use complex named queries), make sure that code is worthwhile after the cache comes into play. Even if it seems that using a simple named query will be faster to load data, consider what would happen in the long run if those entities were loaded into the L2 cache via a call to the find() method.

## Sizing the JPA cache

As with all cases where objects are reused, the JPA cache has a potential performance downside: if the cache consumes too much memory, it will cause GC pressure. This may require that the cache be tuned to adjust its size or that you control the mode in which entities remain cached. Unfortunately, these are not standard options, so you must perform these tunings based on which JPA provider you are using.

JPA implementations typically provide an option to set the size of the cache, either globally or per entity. The latter case is obviously more flexible, though it also requires more work to determine the optimal size for each entity. An alternative approach is for the JPA implementation to use soft and/or weak references for the L2 cache. EclipseLink, for example, provides five cache types (plus additional deprecated types) based on various combinations of soft and weak references. That approach, while potentially easier than finding optimal sizes for each entity, still requires some planning: in particular, recall from Chapter 7 that weak references do not really survive any GC operation and are hence a questionable choice for a cache.

If a cache based on soft or weak references is used, the performance of the application will also depend on what else happens in the heap. The examples of this section all used a large heap so that caching the 400,896 entity objects in the application would not cause issues with the garbage collector. Tuning a heap when there are large JPA L2 caches is quite important for good performance.

![](img/1711f142ae3cfd740ef106afbbba2046_374_0.png)

## Quick Summary

- The JPA L2 cache will automatically cache entities for an application.
- The L2 cache does not cache entities retrieved via queries. This means that in the long run it can be beneficial to avoid queries altogether.
- Unless query caching is supported by the JPA implementation in use, using a JOIN query turns out to frequently have a negative performance effect, since it bypasses the L2 cache.

## Spring Data

Although JDBC and JPA are standard parts of the Java platform, other third-party Java APIs and frameworks manage database access. NoSQL vendors all have their own APIs to access their databases, and various frameworks provide database access via different abstractions than JPA.

The most-widely used of these is Spring Data, which is a collection of database access modules for both relational and NoSQL databases. This framework contains several modules, including these:

### Spring Data JDBC

This is designed as a simple alternative to JPA. It provides a similar entity mapping as JPA but without caching, lazy loading, or dirty entity tracking. This sits on top of standard JDBC drivers so it has wide support. That means you can track the performance aspects from this chapter in the Spring code: make sure to use prepared statements for repeated calls, implement the necessary interfaces in your code to support Spring batched statement models, and/or work directly with the connection objects to change autocommit semantics.

### Spring Data JPA

This is designed as a wrapper around standard JPA. One large benefit is that it reduces the amount of boilerplate code developers need to write (which is good for developer performance but doesn’t really impact the performance we’re discussing). Because it wraps standard JPA, the JPA performance aspects mentioned in this chapter all apply: setting up eager versus lazy loading, batching updates and inserts, and the L2 caches all still apply.

### Spring Data for NoSQL

Spring has various connectors for NoSQL (and NoSQL-like) technologies, including MongoDB, Cassandra, Couchbase, and Redis. This somewhat simplifies the NoSQL access, since the techniques to access the store are then the same, though differences in setup and initialization remain.

### Spring Data R2DBC

Spring Data R2DBC, mentioned in Chapter 10, allows asynchronous JDBC access to Postgres, H2, and Microsoft SQL Server databases. It follows the typical Spring Data programming model rather than direct JDBC, so it is similar to Spring Data JDBC: access is via simple entities in repositories, though without the caching, lazy loading, and other features of JPA.
