```
$ az servicebus namespace authorization-rule keys list --resource-group $RES_GROUP --namespace-name $NAMESPACE_NAME --name RootManageSharedAccessKey --query primaryConnectionString wBY=
```

Azure provides open source libraries to send and receive messages to the Azure service in all popular programming languages. The following example builds a simple producer and consumer in Python:

```
from azure.servicebus import ServiceBusClient, ServiceBusMessage

import os
connstr = os.environ['SERVICE_BUS_CONN_STR']
queue_name = os.environ['SERVICE_BUS_QUEUE_NAME']

messages = ["Testing 1","Testing 2", "Testing 3"]

with ServiceBusClient.from_connection_string(connstr) as client:
    s = client.get_queue_sender(queue_name)
    r = client.get_queue_receiver(queue_name)

    print(s,r)

    for m in messages:
        s.send_messages(ServiceBusMessage(m))

    while True:
        print(r.receive_messages(max_message_count=1,max_wait_time=10))
```

In addition to RabbitMQ, readers familiar with AWS SNS and SQS will find Service Bus straightforward and able to meet similar use cases.

# Azure Event Hubs

Azure Event Hubs is designed to provide some of the capabilities of Apache Kafka to meet use cases for partitioning and consumer groups, but in a managed cloud service (see Figure 10-15). Although you lose the visibility into the backend infrastructure that you would have when running Kafka on virtual machines or Kubernetes, you gain tighter integration with additional data services such as Azure Blob Storage, Azure Data Lakes, and Azure Stream Analytics, to name a few. Azure also supports both AMQP 1.0 and Kafka 1.0 protocols to maintain compatibility with client libraries.

![](bbox=[0.25, 0.58, 0.75, 0.75])

Figure 10-15. Azure Event Hubs architecture

Azure Event Hubs has some concepts that transfer over from Azure Service Bus. We will quickly review them here:

*Producers*

These are the clients that send messages to Azure Event Hubs.

*Partitions*