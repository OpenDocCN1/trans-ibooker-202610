# Triggers

After defining the necessary parameters, you can define the following triggers, depending on how you want your function to be initiated.

To define an HTTP trigger, use the following command:

```
--trigger-http
```

An endpoint will be assigned to the function.

To trigger a function on changes to a Cloud Storage bucket, use the following command:

```
--trigger-bucket=$TRIGGER_BUCKET
```

Here, we have the following option:

+   - $TRIGGER_BUCKET: The Google Cloud Storage bucket name. Every change that’s made to the files in this bucket will trigger function execution.

To trigger a function on messages that are arriving in a Pub/Sub queue, use the following command:

```
--trigger-topic=$TRIGGER_TOPIC
```

Here, we have the following option:

+   - $TRIGGER_TOPIC: The name of the Pub/Sub topic. Messages arriving in the queue will trigger this function. The message's content will be passed to the function.

For other sources, such as Firebase, use the following command:

```
--trigger-event=$EVENT_TYPE
```

```
--trigger-resource=$RESOURCE
```

Here, we have the following options:

+   - $EVENT_TYPE: The action that should trigger the function
+   - $RESOURCE: A resource from which the event occurs

Let’s have a look at an example of configuring a trigger from Pub/Sub:

```
gcloud functions deploy hello_pubsub --runtime python37
--trigger-topic mytopic
```