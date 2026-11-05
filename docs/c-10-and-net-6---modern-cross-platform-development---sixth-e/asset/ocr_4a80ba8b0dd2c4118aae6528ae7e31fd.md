```
...
Level: Debug, Event Id: 20000, State: Opening connection to database 'main' on server '/Users/markjprice/Code/Chapter10/WorkingWithEFCore/Northwind.db'.
Level: Debug, Event Id: 20001, State: Opened connection to database 'main' on server '/Users/markjprice/Code/Chapter10/WorkingWithEFCore/Northwind.db'.
Level: Debug, Event Id: 20100, State: Executing DbCommand [Parameters=[@__price_0='?'], CommandType='Text', CommandTimeout='30']
SELECT "p"."ProductId", "p"."CategoryId", "p"."UnitPrice",
"p"."Discontinued", "p"."ProductName", "p"."UnitsInStock"
FROM "Products" AS "p"
WHERE "p"."UnitPrice" > @__price_0
ORDER BY "product"."UnitPrice" DESC
...
```

![](bbox=[0.12, 0.36, 0.2, 0.44])

Your logs might vary from those shown above based on your chosen database provider and code editor, and future improvements to EF Core. For now, note that different events like opening a connection or executing a command have different event ids.

# Filtering logs by provider-specific values

The event id values and what they mean will be specific to the .NET data provider. If we want to know how the LINQ query has been translated into SQL statements and is executing, then the event Id to output has an Id value of 20100:

+   1. Modify the Log method in ConsoleLogger to only output events with an Id of 20100, as highlighted in the following code:

```
public void Log<TState>(LogLevel logLevel, EventId eventId,
    TState state, Exception? exception,
    Func<TState, Exception, string> formatter)
{
    if (eventId.Id == 20100)
    {
        // Log the level and event identifier
        Write("Level: {0}, Event Id: {1}, Event: {2}",
            logLevel, eventId.Id, eventId.Name);

        // only output the state or exception if it exists
        if (state != null)
        {
            Write($", State: {state}");
        }
    }
```