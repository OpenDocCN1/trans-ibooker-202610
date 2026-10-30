
To summarize this, let's look at how this works in practice in the preceding diagram.
When all of the deliveries are complete, the message will be removed from the queue.

As Cloud Pub/Sub integrates well with other GCP products, it opens up a lot of use cases.

Let's have a look at the most common use cases:

- **Distributed event notifications:** Let's take a service that needs to send a notification whenever there is a new registration. Pub/Sub makes it possible for a downstream service to subscribe and receive notifications of this new registration.
- **Balancing workloads:** Tasks can be distributed among multiple Compute Engine instances.
- **Logging:** Pub/Sub can write logs to multiple systems. For example, if we wish to work with real-time information, we could write to a monitoring system, and if we wish to analyze the data at a later time, we could write to a database.

## Creating a topic and a subscription

Pub/Sub can ingest data from several data sources. The following diagram shows the key services that can pass data to Pub/Sub. This is the first step in our solution:

![](img/de2c97ab017f36b3750a5bade111edd7_396_0.png)

Figure 13.3 – Passing data to Pub/Sub

Now, let's look at creating a Pub/Sub topic and subscription. Then, we will publish a message and pull it via Cloud Shell. Please refer to Chapter 16, Google Cloud Management Options, for more information on Cloud Shell. At this point, we are assuming you can open this:

1. From the **Navigation** menu, browse to **BIG DATA | Pub/Sub**, as shown in the following screenshot:

Figure 13.4 – Selecting Pub/Sub

2. Click on CREATE TOPIC:

![](img/de2c97ab017f36b3750a5bade111edd7_397_0.png)

Figure 13.5 – Creating a topic

3. Provide a Topic ID. In this example, we will name it newTopic. Click CREATE TOPIC, as shown in the following screenshot:

### Create a topic

A topic forwards messages from publishers to subscribers.

Topic ID *
newTopic
Topic name: projects/eighth-effect-319712/topics/newTopic

Add a default subscription

Use a schema

Use a customer-managed encryption key (CMEK)

CANCEL   CREATE TOPIC

Figure 13.6 – Entering a Topic ID

4. Select Create subscription, as shown in the following screenshot:

| Topic ID | Encryption key | Topic name | Labels |
| newTopic | Google-managed | projects/eighth-effect-319712/topics/newTopic | — |
Create subscription

Figure 13.7 – Create subscription

5. Provide a **Subscription ID**. In this example, we will call it newSub. We will leave the rest of the settings as their default settings, but we should note what each of these is:

- **Message retention duration**: Pub/Sub will try to deliver the message within a maximum of 7 days. After that, the message will be deleted and is no longer accessible.
- **Subscription expiration**: If there are no active connections or pull/push successes, then the subscription expires.
- **Acknowledgment Deadline**: The subscriber should acknowledge the message within a specific timeframe; otherwise, Pub/Sub will attempt to deliver this message.

Notice that we keep **Delivery Type** set to **Pull**, as shown in the following screenshot:

### Delivery type ?

○ Pull
○ Push

### Message retention duration ?

Duration is from 10 minutes to 7 days

Days 7
Hours 0
Minutes 0

☐ Retain acknowledged messages ?
When enabled, acknowledged messages are retained for the message retention duration specified above. This increases message storage fees. Learn more

### Expiration period ?

○ Expire after this many days of inactivity (up to 365)

31 Days

A subscription is inactive if there is no subscriber activity such as open connections, active pulls, or successful pushes.

○ Never expire
The subscription will never expire no matter the activity.

### Acknowledgement deadline ?

10 Seconds

Deadline time is from 10 seconds to 600 seconds

Figure 13.8 – Subscription settings

6. Browse back to Topics and click on the topics we recently created. Scroll to the bottom, select the MESSAGES tab, and select PUBLISH MESSAGE:

![](img/de2c97ab017f36b3750a5bade111edd7_399_0.png)

Figure 13.9 – Browsing to messages

7. Enter the message's content and select PUBLISH, as shown in the following screenshot:

### Message body

The message you want to publish to this topic. Either message or attribute will be required to publish.

![](img/de2c97ab017f36b3750a5bade111edd7_399_1.png)

### Message attributes

At most 100 attributes per message, attribute key should not start with 'goog' and should not exceed 256 bytes

![](img/de2c97ab017f36b3750a5bade111edd7_399_2.png)

Figure 13.10 – Publishing a message

8. From Cloud Shell, we can now pull down the message:

```
gcparchitectbook@cloudshell:~ (eighth-effect-319712)$ gcloud pubsub subscriptions pull --auto-ack newSub
```

| DATA | MESSAGE_ID | ORDERING_KEY | ATTRIBUTES | DELIVERY_ATTEMPT |
| Good Luck in the exam !! | 2629643884009593 |  |  |  |

Figure 13.11 – Cloud Shell output

This concludes how to publish and retrieve messages to and from Pub/Sub.

## Pub/Sub Lite

While Pub/Sub should be our default solution for most application integration use cases, GCP does offer Pub/Sub Lite for applications when a lower cost justifies some operational overhead. It offers fewer features, lower availability, and there is a requirement to manually reserve and manage resource capacity. However, using Pub/Sub Lite for a system with a single subscription could be up to 85% cheaper.

Some of the other main differences between the products are as follows:

- Pub/Sub routes messages globally, whereas Lite routes messages zonally.
- Pub/Sub topics and subscriptions are global resources, whereas with Lite, they are zonal resources.
- Pub/Sub scales automatically, whereas Lite requires us to manually provision capacity.

## IAM

Access to Pub/Sub is secured with IAM. Let’s have a look at the list of predefined roles, along with a short description of each:

- **Pubsub Publisher**: This has permission to publish a topic.
- **Pubsub Subscriber**: This has permission to consume a subscription or attach a subscription to a topic.
- **Pubsub Viewer**: This has the right to list topics and subscriptions.
- **Pubsub Editor**: This has the right to create, delete, and update topics, as well as create, delete, and update subscriptions.
- **Pubsub Admin**: This has the right to retrieve and set IAM policies for topics and subscriptions.

## Quotas and limits

Pub/Sub comes with some limits. We recommend that you are aware of the limits for each service as this can have an impact on your scalability. For Pub/Sub, we should be aware of the following limits:

- There is a limit of 10,000 topics and 10,000 subscriptions per project.
- There is a limit of 10,000 attached subscriptions per topic.
- There is a limit of 1,000 messages and a 10 MB total size per publishing request.
- There is a limit of 10 MB per message.

## Pricing

Pub/Sub is billed per message ingestion and delivery. There is also a storage charge for retained acknowledged messages.

In this section, we looked at Pub/Sub and Pub/Sub Lite. In the next section, we'll look at Cloud Dataflow.

## Cloud Dataflow

Cloud Dataflow is a service based on Apache Beam, which is an open source software for creating data processing pipelines. A pipeline is essentially a piece of code that determines how we wish to process our data. Once these pipelines have been constructed and input into the service, they become a Dataflow job. This is where we can process the data that's been ingested by Pub/Sub. It will perform steps to change our data from one format to another and can transform both real-time streams or historical batch data. Dataflow is completely serverless and fully managed. It will spin up the necessary resources to execute our Dataflow job and then delete these resources when the job is complete. As an example, a pipeline job might be made up of several steps. If a specific step needs to be executed on 15 machines in parallel, then Dataflow will automatically scale to these 15 machines and remove them when the job is complete. These resources are based on Compute Engine and are referred to as workers, and Cloud Storage is used as a temporary staging area and I/O. These resources are based on our location settings.

> **Exam Tip**
> Dataflow will transform and enrich data in stream and batch modes. Its serverless architecture can be used to shard and process very large batch datasets or a high volume of live streams in parallel.

Cloud Dataflow is the next stage of our end-to-end solution and will process the data that's passed by Cloud Pub/Sub. Let's look at how this impacts our service designs from Figure 13.3:

![](img/de2c97ab017f36b3750a5bade111edd7_401_0.png)
