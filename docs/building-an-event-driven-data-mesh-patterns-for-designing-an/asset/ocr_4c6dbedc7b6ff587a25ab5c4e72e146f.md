CDC connectors are a significant improvement over query-only connectors. They reduce the barrier to bootstrapping data products and make it easier to get started with event streams. However, there still remains room for improvement, particularly as regards further isolating the internal data model from that which is published to the external world. Let’s take a look at the transactional outbox pattern next.

# ## Change-Data Capture Using a Transactional Outbox

A *transactional outbox* is a dedicated database table that acts as a temporary output buffer for events to be written to the event stream. When you update your *internal domain model*, you select only the data that you want to expose to the outside world and write it to the outbox. Then, a separate asynchronous process, such as a dedicated CDC connector, consumes the data from the outbox and writes it to the event stream. Figure 8-2 shows the end-to-end process.

![](bbox=[0.143, 0.346, 0.857, 0.528])

*Figure 8-2. Getting events from a database using a transactional outbox and a dedicated connector*

The transactional outbox pattern is a more invasive alternative to simply capturing existing internal model tables because it requires altering both the database and the existing code. First, we’ll need to create an outbox for the data we want to turn into events. The most common option is to create one table per domain entity and enforce data types based on the table’s schema.

Secondly, we’ll need to modify the application code by wrapping relevant internal entity updates in a *transaction*. Your database must support transactions for this pattern to work. Within the transaction, you will need to select the data you want to expose and write it to the outbox. Finally, you can close and commit the transaction. Updates to the internal domain model either happen atomically or not at all.

The Python code of Example 8-1 illustrates an atomic update of an `EcomItem` in a MySQL database. The internal model update is executed prior to the transactional outbox update, though both are wrapped within a single transaction for consistency.