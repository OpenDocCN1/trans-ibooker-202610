a. When you publish a message to Pub/Sub, Pub/Sub stores the message in the nearest Google Cloud region. Pub/Sub then delivers the message to subscribers across the world.

```
If you need to restrict the regions you can run the following command which restricts the region to us-central1: gcloud pubsub topics update [YOUR_TOPIC] --message-storage-policy-allowed-regions=us-central1
```

The next step is to prepare your application code to be pushed to the master branch in your GitLab repository.

+   1. Git clone the following repository: to come
+   2. Copy the folder in the cloned repository saf-longrun-job-func to your GitLab repository
+   3. Commit your changes and push the code to your repository
+   4. If you head back to GitLab CI/CD you should see your job running:
+   5. You should see the following message once the Pipeline has successfully executed:
+   6. Open the Cloud Function in the Google Cloud Console:
+   7. You should see the Cloud Function successfully deployed.
+   8. Click the on the function name you deployed and then click on the Trigger tab, this will list the configuration details for your trigger type and the associated resource which was defined the GitLab yml file as:
+   9. Before you can upload the audio file you need to enable the Cloud Speech-to-Text API. The cloud function will send the audio file to the Speech-to-Text API and it

![](bbox=[0.16, 0.43, 0.7, 0.46])

![](bbox=[0.17, 0.52, 0.83, 0.56])

![](bbox=[0.18, 0.61, 0.42, 0.64])

```
gcloud --quiet --project $PROJECT_ID functions deploy safLongRunJobFunc --runtime=nodejs8 --trigger-event=google.storage.object.finalize --trigger-resource=$TRIGGER_RESOURCE
```