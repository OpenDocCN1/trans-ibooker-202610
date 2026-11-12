with an application written in, for example, Visual Basic 6. You’ll either need to modernize the application or use the underlying database as an integration point.

# Database-generated events

Generating events by reading changes made to the application database is an alternative to using APIs or services.5 Reading changes can be done via polling, triggers, or change data capture (CDC). This model of distributing events, as you can imagine, has a stronger relationship with event-carried state transfer. Just like in the scenario in which applications generate events, more components and further work may be required.

The first method is a polling-based approach using database connectors. Database tables are mapped to message queues or event streams by repeatedly running queries (typically via Open Database Connectivity or Java Database Connectivity). Each time a new record is inserted into a table, or a row is updated, it’s fetched from the table and transferred to either a message queue or an event stream. The polling-based method is relatively easy to set up but might generate a higher load on the database because of the constant querying. Another drawback of polling is that you might need to implement changes to the application and underlying database design. A prerequisite, for example, could be an extra updated_at column that is required in order to detect what new rows need to be fetched from the database.

An alternative to polling is to generate events using a trigger-based approach. A trigger is a piece of procedural code, like a stored procedure or database hook, that is only executed when a given event happens. For example, you might have a trigger that is executed each time data is added to a table. This trigger then would either move the records to an events table or change feed, or invoke a function to send the data to another component. A problem with triggers is that they can run slowly when a database is under heavy load. Another drawback is that not all databases support triggers.

Another method for capturing the state is to use CDC.6 This works by using specialized database tooling and reading every single change from the database transaction or audit log. CDC-based tools utilize the database less because they don’t query the database directly, but in many cases they require other commercial or third-party components.

Capturing data directly from application databases isn’t easy. Many applications are designed for operational workloads. They work with complex and heavily normalized

5 This form of state is also known as resource state, or the current state of a resource on a server at any point in time. Some engineers distinguish between application state, which lives on the client, and resource state, which lives on the server.

6 An example of using CDC with Azure SQL and Debezium can be found in the Microsoft documentation.