# Structure of a KQL Statement

The syntax of a KQL statement is very much like the language you’d use to tell your colleague how to build a query. It could sound something like this:

> From the Weather table, take the data only from the last 100 days and only the first 200 rows. Aggregate these rows by <aggregation type> on <column> and group the results by <columns>.

Directly translated, this requirement will look like this in KQL:

```
Weather
| where StartTime >= ago(100d)
| take 200
| summarize
    sum(DamageCrops)
    by State
```

From this KQL statement, you can see that the narrative fits the syntax of the query structure. Start with the table name (Weather) and filter (with where) on the StartTime column and only the last 100 days. From this, take the 200 rows and summarize the sum of DamageCrops by State.

As you can see, the KQL language is a script language that works and sounds very much like how you would describe your needs to the developer. But you still need to understand how to write in this language, and that’s what this section will teach you.

# Starting a New Line of Code

You must start every new line of code with a pipe (|) to tell the Kusto engine that you’re writing a new task in the statement. This also means that you can begin to structure