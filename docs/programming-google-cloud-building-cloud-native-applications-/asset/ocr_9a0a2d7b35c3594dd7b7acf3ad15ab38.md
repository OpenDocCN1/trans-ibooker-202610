existing job and click the stop button. STOP . When prompted choose to Drain the job Drain .

+   1. Start the pipeline, this time you are passing 5 as the window size telling the window to be set at 5-minute intervals.

```
python pub_sub_cloud_storage.py \
--project=$PROJECT_NAME \ --
input_topic=projects/$PROJECT_NAME/topics/chapter4-topic \ --output_path=gs://$BUCKET_NAME/samples/output \ --runner=DataflowRunner \
--region=us-central1 \
--window_size=5 \
--temp_location=gs://$BUCKET_NAME/temp
```

+   2. To view the files written to Cloud Storage run the following command on the terminal to list the files:

```
gsutil ls gs://$BUCKET_NAME/samples
```

+   3. Notice the timestamp of each file, they are now set at intervals of 5 minutes (Figure 4-16).

![](bbox=[0.143, 0.545, 0.856, 0.838])

Figure 4-16. Five Minute Dataflow Windowing Output