![](bbox=[0.12, 0.08, 0.86, 0.25])

Figure 9-8. Unifying incoming events

Model transformation that aggregates incoming data cannot be implemented using an API gateway, and thus requires more elaborate, stateful processing. To track the incoming data and process it accordingly, the translation logic requires its own persistent storage (see Figure 9-9).

![](bbox=[0.14, 0.4, 0.86, 0.6])

Figure 9-9. Stateful model transformation

In some use cases, you can avoid implementing a custom solution for a stateful translation by using off-the-shelf products; for example, a stream-process platform (Kafka, AWS Kinesis, etc.), or a batching solution (Apache NiFi, AWS Glue, Spark, etc.).

# Unifying multiple sources

A bounded context may need to process data aggregates from multiple sources, including other bounded contexts. A typical example for this is the backend-for-frontend pattern,¹ in which the user interface has to combine data originating from multiple services.