# Defining and Using Common Entities

One of the first important steps is to define the minimal entities that are used across many areas of your business. Let’s take a look at an example first.

An ecommerce company defines a common Item entity containing two fields: long id and long upc_code. Data product owners are expected to use the Item entity in any data product that references their ecommerce items, be it Order, Inventory entry, or Return. Each of these related entities uses a common and standardized version of Item, removing the need for consumers to interpret similar-yet-different representations of the same data.

Common entities do not preclude you from adding more information about that entity to your data product. You are free to extend your data products to include other information about Item, such as size and color in the case of a clothing item or weight and serving_size in the case of a food item. Think of a common entity as an attachment point between data products in other domains and as an extendable base for the entity’s data model.

# Event Stream Keying and Partitioning

Interoperability of event-streaming data products is affected by the partition count of the stream, the key of the event, and the partition assignment algorithm (see Chapter 3). An event stream contains one or more partitions, and each event is assigned to a partition based on the partition assignment algorithm, the event key, and the number of partitions. Here are some useful interoperability tips:

# Partition count

Joining and aggregating data products from multiple streams can be made much less computationally intensive if the event partition counts are the same size. While event-stream processors like Kafka Streams and Flink can automatically repartition event streams as needed, it requires more processing power and can incur higher costs. Try using a T-shirt sizing approach to standardize partition counts, such as x-small=1, small=4, medium=8, large=16, x-large=32, xx-large=64, jumbo=256. As part of your self-service platform (which we’ll cover in the next chapter), you can provide the data mesh users with instructions for choosing partition count based on the key space, volume of events, and consumer reprocessing needs.

![](bbox=[0.203, 0.783, 0.28, 0.868])

If you’re building a data product keyed on a common entity, check the partition count of other data products also keyed on that entity—if they’re all using the same partition count, you would do well to use it, too.