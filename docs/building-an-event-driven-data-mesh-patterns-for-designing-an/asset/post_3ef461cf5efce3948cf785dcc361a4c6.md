
CDC connectors are a significant improvement over query-only connectors. They reduce the barrier to bootstrapping data products and make it easier to get started with event streams. However, there still remains room for improvement, particularly as regards further isolating the internal data model from that which is published to the external world. Let’s take a look at the transactional outbox pattern next.

## Change-Data Capture Using a Transactional Outbox

A *transactional outbox* is a dedicated database table that acts as a temporary output buffer for events to be written to the event stream. When you update your *internal domain model*, you select only the data that you want to expose to the outside world and write it to the outbox. Then, a separate asynchronous process, such as a dedicated CDC connector, consumes the data from the outbox and writes it to the event stream. Figure 8-2 shows the end-to-end process.

![](img/4a76b8ac0abd2ab4b0aad729485a0a2b_195_0.png)

*Figure 8-2. Getting events from a database using a transactional outbox and a dedicated connector*

The transactional outbox pattern is a more invasive alternative to simply capturing existing internal model tables because it requires altering both the database and the existing code. First, we’ll need to create an outbox for the data we want to turn into events. The most common option is to create one table per domain entity and enforce data types based on the table’s schema.

Secondly, we’ll need to modify the application code by wrapping relevant internal entity updates in a *transaction*. Your database must support transactions for this pattern to work. Within the transaction, you will need to select the data you want to expose and write it to the outbox. Finally, you can close and commit the transaction. Updates to the internal domain model either happen atomically or not at all.

The Python code of Example 8-1 illustrates an atomic update of an `EcomItem` in a MySQL database. The internal model update is executed prior to the transactional outbox update, though both are wrapped within a single transaction for consistency.

### Example 8-1. Atomic update of the internal model and the transactional outbox

```
try:
    conn = mysql.connector.connect(host='localhost',
                                    database='python_db',
                                    user='abellemare',
                                    password='definitelynotpassword')
    conn.autocommit = False
    cursor = conn.cursor()

    # Perform the internal domain model update
    internal_model_update = """
        Update EcomItem
        set price = 1299.99
        where id = 4291"""
    cursor.execute(internal_model_update)

    # Select the subdomain of the internal model we want to write to
    # the transactional outbox
    internal_sub_model_query = """
        Select name, price
        from EcomItem
        where id = 4291"""
    cursor.execute(internal_sub_model_query)
    name_and_price = cursor.fetchone()

    if (name_and_price == None):
        raise Exception("Unexpected missing record. Can't get name_and_price")

    # Insert the selected data into the outbox
    outbox_insert = """INSERT INTO EcomItem_Outbox (id, name, price)
        VALUES (4291, %s, %s)"""

    # Pass the name and price in to replace the query wildcards
    cursor.execute(outbox_insert, name_and_price)

    # Commit the internal and outbox updates atomically
    conn.commit()

except mysql.connector.Error as error:
    # reverting changes because of exception
    conn.rollback()

finally:
    # Close the database connection
    if conn.is_connected():
        cursor.close()
        conn.close()
```

In this example, we first update the EcomItem price to $1299.99. Next, we select the name and price from the table we just updated—we are forced to query name at the very least since we want to produce a state-based event that contains all of the current state for the item we just updated. Finally, we compose the event and write it into the EcomItem_Outbox format, as shown in Table 8-1, just as we would any other relational database table.

### Table 8-1. EcomItem_Outbox table definition

| id | name | price | Datetime |
|---|---|---|---|
| 4291 | "Fancy Laptop" | 1299.99 | 2022-06-22 11:33:12 |

There are two things to note about our SQL table definition. One, we are using NOT NULL for each of the mandatory fields that we expect in our event. Inserting a new event into the outbox will fail unless all constraints are met. Two, we’re populating the Datetime field by default using CURRENT_TIMESTAMP if it is not provided by the application’s code. You may use this timestamp as part of the schema for your event, or you may use it to populate the event’s metadata to indicate when the event was created.

Ensure that the internal model, the transactional outbox table model, and the event schema are compatible. Use pre-deployment to ensure that internal model updates correctly transcribe to the outbox table model and event schema to avoid easily preventable runtime errors.

Obtaining data from the outbox can be accomplished using exactly the same techniques we’ve talked about earlier in this chapter: either by directly querying the outbox via a polling connector or by using a CDC connector.

You’ll also need to clean up the outbox table from time to time. If you’re using a query-based polling mechanism, cleanup is as simple as deleting each of the rows from the outbox as they are written to the event stream.

Debezium offers a full-featured outbox to Kafka topic mechanism known as the outbox event router. It prevents propagation of DELETE events from outbox tables but can still send out tombstones by setting route.tombstone.on.empty.payload to true. Additionally, it will automatically clean up the outbox table to prevent it from growing unbounded.

You should use off-the-shelf, freely available products such as Debezium whenever possible to simplify your data mesh journey. A great many developer hours and testing hours have gone into hardening it, and it’ll provide you with a far more robust solution than most home-grown solutions. You’d be better off spending your efforts on building your data products and establishing a self-service platform than on trying to reinvent CDC solutions.

Some database storage engines allow you to create a table that doesn’t save the data to the database but instead writes it to the database’s log. MySQL’s BLACKHOLE storage engine simply discards all writes but preserves all INSERT, UPDATE, and DELETE operations to the log. The BLACKHOLE storage engine requires minimal disk IO and it absolves you of having to clean up the table since nothing is ever written to it.

There are a number of pros to using a transactional outbox:

- Internal data model isolation: The outbox provides isolation of the internal data model from the external data model, allowing both to evolve independently.
- Exactly-once outbox semantics: Transactions ensure that both the internal model and the outbox data are created together or not at all. A failure to update the internal model will prevent an event from being written to the outbox, but so will the reverse. Test your code and exercise each code path.
- Early schema enforcement: The outbox table schema determines the event schema. All events written to the outbox will comply with the event stream schema requirements, significantly reducing the chance for errors.
- Denormalized data: You can choose to denormalize data during the transaction, such that downstream consumers do not have to join the data themselves. We’ll go into this more in “Denormalization and Eventification” on page 186.

Producing events via a transactional outbox also has several cons:

- Database must support transactions: It bears repeating that your database must support transactions. If it does not, you’ll have to choose a different data access pattern.
- Application code updates: The application code must be updated to enable this pattern, which requires development and testing resources.
- Outbox write may fail the transaction: A transaction failure due to a write issue to the outbox will also cause the write to the internal model to fail. This may be unacceptable for your use case, but is easily prevented by comprehensively testing your code and validating your transactional updates prior to deploying your code.
