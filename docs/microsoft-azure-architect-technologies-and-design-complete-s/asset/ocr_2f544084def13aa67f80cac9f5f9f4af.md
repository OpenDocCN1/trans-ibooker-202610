# **FIGURE 5.38** Using the Data Explorer in the Azure portal to view Azure Cosmos DB and containers

![](bbox=[0.246, 0.154, 0.742, 0.399])

From an architectural context, the container is the most important element in this group. It’s most important because it is the location where scaling occurs and the SLA is applied. You can see in Figure 5.38 that the scale settings are managed at a level under the container name. You will also see on the Overview blade of the Azure Cosmos DB account, as shown in Figure 5.39, that the number of RU/s are applied to the container and not to the database or account. You might be asking yourself about the term RU. A *request unit (RU)* is the unit of throughput that identifies the amount of available compute resources available to the container (i.e., CPU, memory, and IOPS). You might remember DTUs from the previous section; it is a similar unit of measure. Like the DTU, an RU is what the cost of the product is; the more you need the more it costs.

# **FIGURE 5.39** Different scale settings per Azure Cosmos DB container

| ID | Database | Throughput (RU/s) |
|---|---|---:|
| csharpguitar-container | csharpguitar-db | 800 |
| csharpguitar-container | csharpguitar-cosmos-db | 500 |
| container-csharpguitar | csharpguitar-cosmos-db | 400 |

To calculate the cost, know that the cost is based on the RUs utilized per hour, plus the amount of storage you consume. It is important to understand that when horizontal scaling happens, the number of RUs are not increased; rather, you receive a new instance of that container in another geographical location. However, if you know that you need 900