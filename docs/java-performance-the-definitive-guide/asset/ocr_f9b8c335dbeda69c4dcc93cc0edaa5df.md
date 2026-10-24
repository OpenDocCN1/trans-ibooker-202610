# Default caching (lazy loading)

In the sample code, the stock prices are loaded via a named query. In the default case, this simple query is executed to load the stock data:

```
@NamedQuery(name="findAll",
    query="SELECT s FROM StockPriceImpl s ORDER BY s.id.symbol")
```

The StockPrice class has a @OneToMany relationship with the StockOptionPrice class using the optionsPrices instance variable:

```
@OneToMany(mappedBy="stock")
private Collection<StockOptionPrice> optionsPrices;
```

@OneToMany relationships are loaded lazily by default. Table 11-3 shows the time to execute this loop.

Table 11-3. Seconds required to read data for 256 stocks (default configuration)

| Test case | First execution | Subsequent executions |
|---|---|---|
| Lazy relationship | 22.7 ± 2 seconds (66,817 SQL calls) | 1.1 ± 0.7 seconds (1 SQL call) |
| Lazy relationship, no traversal | 2.0 ± 0.3 seconds (1 SQL call) | 1.0 ± 0.02 seconds (1 SQL call) |

The first time the sample loop is executed in this scenario (for 256 stocks with one year of data), the JPA code executes one SQL statement in the call to the executeQuery() method. That statement is executed at SQL Call Site 1 in the code listing.

As the code loops through the stock and visits each collection of option prices, JPA will issue SQL statements to retrieve all the options associated with the particular entity (that is, it retrieves the entire collection for one stock/date combination at once). This occurs at SQL Call Site 2, and it results in 66,816 individual SELECT statements during execution (261 days × 256 stocks), yielding 66,817 total calls.

That example takes almost 23 seconds for the first execution of the loop. The next time that code is executed, it takes only a little more than 1 second. That's because the second time the loop is executed, the only SQL executed is the named query. The entities retrieved via the relationship are still in the L2 cache, so no database calls are needed in that case. (Recall that the L2 cache works only for entities loaded from a relationship or a find operation. So the stock option entities can be found in the L2 cache, but the stock prices—since they were loaded from a query—do not appear in the L2 cache and must be reloaded.)

The second line in Table 11-3 represents the code that does not visit each of the options in the relationship (i.e., the processOptions variable is false). In that case, the code is substantially faster: it takes 2 seconds for the first iteration of the loop and 1 second for subsequent iterations. (The difference in performance between those