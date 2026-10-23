As you begin learning some Event Grid details, review Table 6.1 and then proceed to learning about these most common Event Grid terms:

+   - Event, publisher, source, and handler
+   - Domain, subscription, and topic

In the previous exercise, recognize that the event (aka notification) comes from the creation of a blob into the monitored source Azure Storage Blob container, as this is where the event happens. Notice again in Figure 6.27 that you selected Blob Created, which instructed the source to send the event notification to Event Grid. Event Grid then sends the event with the details, as shown in Figure 6.28 to the handler—an Azure Function. In the Exercise 6.5 scenario, you have the following:

+   - **Event:** Blob Created
+   - **Source:** Azure Storage
+   - **Handler:** Azure Function

As shown in Figure 6.30, many Azure products have built-in support for configuring and subscribing to Event Grid via the Events link on the navigation menu for the given Azure product.

# FIGURE 6.30  A view of event grid sources and handlers

![](bbox=[0.143, 0.489, 0.846, 0.735])

If the Azure product you want to send events to is an Event Grid that doesn’t have the built-in ability to subscribe to Event Grid, then you need to manually create the Event Grid Topic. The same goes for the creation of custom events. When you create an Event Grid Topic, it exposes a global endpoint, which is similar to the following:

```
`<topicName>.westeurope-1.eventgrid.azure.net/api/events`
```

The Event Grid Topic exposes an API to which you can send JSON documents that contain information about the event, as well as some data that the Event Grid sends on to the