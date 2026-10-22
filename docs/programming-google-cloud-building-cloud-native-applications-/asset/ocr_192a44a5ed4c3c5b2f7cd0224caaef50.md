# Seek to a timestamp

Another way to replay messages that have been acknowledged is to seek a timestamp. To seek a timestamp, you must first configure the subscription to retain acknowledged messages using retain-acked-messages.

+   1. Run the following command to seek to a timestamp
+   ```
+   gcloud pubsub subscriptions update seek-demo-sub --retain-acked-messages
+   ```
+   2. Now publish, pull, and acknowledge a message:
+   ```
+   gcloud pubsub topics publish seek-demo-topic --message 'hello, world'
+   gcloud pubsub subscriptions pull --auto-ack seek-demo-sub
+   ```
+   3. The next pull should return no messages:
+   ```
+   gcloud pubsub subscriptions pull seek-demo-sub
+   ```
+   4. Now seek the subscription back in time to recover the message
+   ```
+   TS_FORMAT=%Y-%m-%dT%H:%M:%SZ
+   gcloud pubsub subscriptions seek seek-demo-sub --time=$(date -u -d '-10 min' +$TS_FORMAT)
+   ```
+   5. A subsequent pull should yield the message again:
+   ```
+   gcloud pubsub subscriptions pull --auto-ack seek-demo-sub
+   ```
+   6. You can also use seek to skip delivery of all messages published before some point in time.
+   ```
+   gcloud pubsub topics publish seek-demo-topic --message 'hello, world'
+   gcloud pubsub subscriptions seek seek-demo-sub --time=$(date -u +$TS_FORMAT)
+   ```
+   7. After some propagation delay of, say, 60 seconds the message will no longer be delivered.
+   ```
+   gcloud pubsub subscriptions pull seek-demo-sub
+   ```

# Monitoring

Monitoring Cloud Pub/Sub is a great way to keep your application and Cloud Pub/Sub healthy. Cloud Pub/Sub logs metrics to Cloud Monitoring. With Cloud Monitoring you can create dashboards to keep track of the metrics collecting allowing you to create alerts if things start to go wrong as making sure subscribers are keeping up with the flow of messages.

To access Cloud Monitoring:

+   1. Open the Monitoring in the Google Cloud Console:
+      Monitoring