
```
$ az servicebus namespace authorization-rule keys list --resource-group $RES_GROUP --namespace-name $NAMESPACE_NAME --name RootManageSharedAccessKey --query primaryConnectionString
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

## Azure Event Hubs

Azure Event Hubs is designed to provide some of the capabilities of Apache Kafka to meet use cases for partitioning and consumer groups, but in a managed cloud service (see Figure 10-15). Although you lose the visibility into the backend infrastructure that you would have when running Kafka on virtual machines or Kubernetes, you gain tighter integration with additional data services such as Azure Blob Storage, Azure Data Lakes, and Azure Stream Analytics, to name a few. Azure also supports both AMQP 1.0 and Kafka 1.0 protocols to maintain compatibility with client libraries.

![](img/8d47d1b5dbca55e784ae602145a894c7_349_0.png)

Figure 10-15. Azure Event Hubs architecture

Azure Event Hubs has some concepts that transfer over from Azure Service Bus. We will quickly review them here:

*Producers*

These are the clients that send messages to Azure Event Hubs.

*Partitions*

Messages are sharded across partitions to enable replicas of the message to be stored.

### Consumer groups

These are clients that work in unison to consume messages produced in a coordinated manner.

### Managing Azure Event Hubs with Terraform

The Terraform code necessary to provision the required resources to send messages to Azure Event Hubs closely resembles what we previously used to configure a queue and topic within Azure Service Bus. We must declare a resource group and a namespace that our Event Hubs instance will be part of:

```
provider "azurerm" {
  features {}
}

resource "azurerm_resource_group" "example" {
  name     = "mdfranz-eventbus-group"
  location = "East US"
}

resource "azurerm_eventhub_namespace" "example" {
  name                = "mdfranz-eventhub-namespace"
  location            = "East US"
  resource_group_name = azurerm_resource_group.example.name
  sku                 = "Standard"
  capacity            = 1
}

resource "azurerm_eventhub" "example" {
  name                = "mdfranz-eventhub"
  namespace_name      = azurerm_eventhub_namespace.example.name
  resource_group_name = azurerm_resource_group.example.name
  partition_count     = 1
  message_retention   = 1
}

resource "azurerm_eventhub_consumer_group" "example" {
  name                = "mdfranz-consumergroup"
  resource_group_name = azurerm_resource_group.example.name
  namespace_name      = azurerm_eventhub_namespace.example.name
  eventhub_name       = azurerm_eventhub.example.name
}
```

Creating a consumer group is optional as there is a default consumer group. Note also that having multiple consumer groups does require the standard SKU as opposed to the basic SKU.

As was the case with Azure Event Bus, there are multiple ways to authenticate to Event Hubs endpoints. The easiest way is to use a connection string, as shown in the following example:

```
$ az eventhubs namespace authorization-rule keys list --resource-group mdfranz-eventbus-group --namespace-name mdfranz-eventhub-namespace --name RootManageSharedAccessKey
{
  "aliasPrimaryConnectionString": null,
  "aliasSecondaryConnectionString": null,
  "keyName": "RootManageSharedAccessKey",
  "primaryConnectionString": "Endpoint=sb://mdfranz-eventhub-namespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=jFMUbfQZX8UjGjcCKLAeIp5CZVLcWcSGpuKdJX/CMzK=",
  "primaryKey": "jFMUbfQZX8UjGjcCKLAeIp5CZVLcWcSGpuKdJX/CMzK=",
  "secondaryConnectionString": "Endpoint=sb://mdfranz-eventhub-namespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=gFy7SuuGjw12GfoHXd6UQeZdy5Y0xu/gStZtQUhaxsY=",
  "secondaryKey": "gFy7SuuGjw12GfoHXd6UQeZdy5Y0xu/gStZtQUhaxsY="
}
```

> NOTE
> The parameter EntityPath=mdfanz-eventhub (i.e., Endpoint=sb://mdfanz-eventhub-namespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;EntityPath=mdfanz-eventhubSharedAccessKey=jfMUbFqZX8UjG1cKLAeIp5CZVLcwcSGpuKdJX/CMzk=) must be added to the connection string, because if you are creating a client using NewHubfromConnectionString, the Send method does not allow an event grid name to be specified as a parameter, so you need to add an EntityPath instead.

## Azure Event Grid

Azure Event Grid was released for general availability in 2018 and is the best of the Azure-offered messaging brokers, as it allows multiple event sources to be routed to a variety of destinations within the Azure ecosystem. Messages can be sent to/from the existing message services we just discussed (Azure Service Bus and Azure Event Hubs) and to additional services and endpoints (shown in Figure 10-16), such as Azure Functions, Web Hooks, Logic Apps, and other in-application purposes (e.g., tracking how users interact with the application). Azure Event Hubs is a first-class “serverless” offering in which billing is by the event.

![](img/8d47d1b5dbca55e784ae602145a894c7_351_0.png)

Figure 10-16. The Event Grid ecosystem

In Figure 10-16, note the push nature of the Event Grid as opposed to the general push-pull topology we’re used to in other systems.

Azure Event Grid uses terminology that slightly differs from the other platforms we’ve discussed:

### Domains

Domains are analogous to namespaces which separate use cases within the tenant.

### Topics

For Event Grids, there are two types of topics: system topics, which come from Azure systems (Event Hubs, Blob Storage, etc.), as shown on the left in Figure 10-16; and custom topics, which can be your own customized scheme wrapped in the Event Grid Schema.

### Deploying and using event grids

Azure event grids have a minimalistic Terraform deployment, as shown in the following example:

```
resource "azurerm_resource_group" "example" {
  name     = "example-resources"
  location = "East US"
}
```

```
resource "azurerm_eventgrid_topic" "example" {
  name                = "my-eventgrid-topic"
  location            = azurerm_resource_group.example.location
  resource_group_name = azurerm_resource_group.example.name

  tags = {
    environment = "Production"
  }
}
```

You will want to change the name of the topic as it is globally unique across the Azure region you deploy to. Once this Terraform deployment is applied, a new Event Grid topic will be created with the name https://<event-grid-topic-name>.<topic-location>.eventgrid.azure.net/api/events; or in our case, https://my-eventgrid-topic.east-us.eventgrid.azure.net/api/events. You will be able to find the access key for the topic by running az eventgrid topic key list --resource-group example-resources --name my-eventgrid-topic.

Here is a simple producer using the azure-eventgrid Python package:

```
from azure.identity import DefaultAzureCredential
from azure.eventgrid import EventGridPublisherClient, EventGridEvent

event = EventGridEvent(
    data={"team": "azure-sdk"},
    subject="Door1",
    event_type="Azure.Sdk.Demo",
    data_version="2.0"
)

credential = DefaultAzureCredential("<access-key>")
endpoint = `https://my-eventgrid-topic.east-us.eventgrid.azure.net/api/events`
client = EventGridPublisherClient(endpoint, credential)
client.send(event)
```

A number of great examples on how to code with the Event Grid are available at the Azure Event Grid client library for Python.

## Summary

You have many choices when picking messaging technologies to implement with new or existing applications. As usual, the answer as to what and how to run a piece of technology is often “it depends!” There is significant overlap in functionality across the open source and Azure messaging offerings we discussed in this chapter. Ensuring that you understand your durability, scale, and security requirements before picking a messaging service will set you up for success as you deploy a messaging infrastructure. We will now move to our final piece of infrastructure, serverless computing.
