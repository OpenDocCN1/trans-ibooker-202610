![](bbox=[0.18, 0.1, 0.8, 0.23])

To run a build request using your Dockerfile, run the following command from the directory containing your application code, Dockerfile, and any other assets:

```
gcloud builds submit --tag us-central1-docker.pkg.dev//[PROJECT_I
```

This builds the image and stores it in Google Artifact Registry.

At this stage we know how to store the Anthos application code, build and host the container image. Now lets have a look how we can trigger the build automatically.

# Cloud Build notifications

There are multiple ways of getting notifications from Cloud Build. You can get notified about any changes in the build state including start, transition and completion of the build.

Cloud Build is well integrated with Pub/Sub and publishes messages to cloud-builds Pub/Sub topic. Both push and pull subscription models are supported. Having the message in the Pub/Sub queue gives you endless options for sending the notifications to the next step.

On top of Pub/Sub you can also get notifications from Cloud Build using one of the following notifications channels.

+   - Slack - posts notifications to a Slack channel
+   - SMTP - emails notifications via SMTP protocol
+   - HTTP - sends notification in JSON format to a HTTP endpoint

All three types of notifications are using containers running as a Cloud Run