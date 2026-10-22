
Congratulations! You have created two Cloud Dataflow jobs with Tumbling windows (called fixed windows in Apache Beam) at 2 and 5-minute intervals. This is a powerful method to perform aggregations across your streaming datasets. Imagine building scorecards, you can allow Dataflow to perform the aggregations as the mean value and write this to a database that will present scorecards to the users. At this point, you don’t need to perform aggregation on the Frontend providing for a fast user experience.

## Watermark

A watermark is a value that you define that will let Dataflow know when to expect the data. If you set the value to 30 seconds, and an element arrives after the 30-second watermark and even though the timestamp is within the window it will be considered late.

For example, if you have a window storing items for events in 30 seconds and you receive the following 3 elements [element 1: 2020-01-09 0:00:01], [element 2: 2020-01-09 0:00:05] & [element 3: 2020-01-09 0:00:15] but element 3 has the event time included in this interval but it comes to the system after the window computation is considered to be on late.

## Trigger

Triggers determine when to emit aggregated results for each window.

Beam provides a number of pre-built triggers that you can set:

### *Event time triggers*

These triggers operate on the event time identified by the timestamp of each element. For example, the AfterWatermark() trigger emits the elements of a window after the watermark passes the end of the window which is based on the timestamps attached to the data elements. The following example uses both early as AfterProcessingTime() and late firings as late=AfterCount(1):

```
AfterWatermark(
    early=AfterProcessingTime(delay=1 * 60),
late=AfterCount(1))
```

### Processing time triggers

These triggers operate on the processing time, the time when the element is processed. The AfterProcessingTime() trigger emits a window after a certain amount of processing time has passed since data was received. The processing time is determined by the system clock, rather than the data element’s timestamp.

### Data-driven triggers

These triggers operate by examining the data as it arrives in each window, and firing when that data meets a certain property. Apache Beam provides one data-driven trigger AfterCount(). It is important to note, for example, you specify AfterCount(25) and only 10 elements arrive, those 10 elements will not be processed since it has not met the condition of 25. If you need those 10 elements, you should consider using composite triggers.

### Composite triggers

These triggers combine multiple triggers. The Apache Beam Documentation notes the following composite triggers:

- You can add additional early firings or late firings to AfterWatermark.pastEndOfWindow via .withEarlyFirings and .withLateFirings.
- Repeatedly.forever specifies a trigger that executes forever. Any time the trigger’s conditions are met, it causes a window to emit results and then resets and starts over. It can be useful to combine Repeatedly.forever with .orFinally to specify a condition that causes the repeating trigger to stop.
- AfterEach.inOrder combines multiple triggers to fire in a specific sequence. Each time a trigger in the sequence emits a window, the sequence advances to the next trigger.
- AfterFirst takes multiple triggers and emits the first time any of its argument triggers is satisfied. This is equivalent to a logical OR operation for multiple triggers.
- AfterAll takes multiple triggers and emits when all of its argument triggers are satisfied. This is equivalent to a logical AND operation for multiple triggers.
- orFinally can serve as a final condition to cause any trigger to fire one final time and never fire again.

## Composition triggers with AfterWatermark()

Useful composite triggers execute once when Beam estimates that all the data has arrived. For example, the following example trigger code executes on the following conditions:

- The watermark passes the end of the window
- Any time late data arrives, after a ten-minute delay
- After two days, we assume no more data of interest will arrive, and the trigger stops executing

```
pcollection | WindowInto(
    FixedWindows(1 * 60),
    trigger=AfterWatermark(late=AfterProcessingTime(10 * 60)),
    allowed_lateness=Duration(seconds=2*24*60*60))  # 2 days,
    accumulation_mode=AccumulationMode.DISCARDING)
```

## Closing Remarks

In this chapter, you learned about Cloud Dataflow. Cloud Dataflow is a fully managed data processing pipeline that allows you to perform transformations at scale on your data. Within this framework, you are leveraging it to enrich speech to text data to provide users analytics of audio recordings. In Chapter 5 you will deploy the second part of the application, and you will continue to build on this application as the chapters progress.

Citations:
https://beam.apache.org/documentation/programming-guide/#data-driven-triggers

## Chapter 5. Deploying the Application Data Pipeline

## A NOTE FOR EARLY RELEASE READERS

With Early Release ebooks, you get books in their earliest form—the author’s raw and unedited content as they write—so you can take advantage of these technologies long before the official release of these titles.

This will be the 5th chapter of the final book. Please note that the GitHub repo will be made active later on.

If you have comments about how we might improve the content and/or examples in this book, or if you notice missing material within this chapter, please reach out to the editor at mcronin@oreilly.com.

In this chapter, you will deploy Cloud Dataflow and learn about Cloud Dataflow monitoring so you can have a healthy pipeline. This will be Part Two of your deployment of the fully functional cloud-native application.

## Deploying the Cloud Dataflow Pipeline

To deploy the Cloud Dataflow pipeline, run the commands below. The pipeline will perform the enrichment of the raw audio data leveraging Google Cloud Machine Learning APIs.

- 1. Create a storage bucket for the Dataflow Staging Files:
  `gsutil mb gs://[BUCKET_NAME]/`
- 2. Through the Google Cloud Console create a folder named tmp in the newly created bucket for the DataFlow staging files

```
gsutil mb gs://[BUCKET_NAME]/
```
- 3. Create a BigQuery Dataset that will hold the structured data after it has been processed by Cloud Dataflow:

```
bq mk [YOUR_BIG_QUERY_DATABASE_NAME]
```
- 4. Enable the Cloud Dataflow API
- 5. gcloud services enable dataflow
- 6. Enable the Cloud Natural Language API:

```
gcloud services enable language.googleapis.com
```
- 7. Enable the Google Cloud Data Loss Prevention API

```
gcloud services enable dlp.googleapis.com
```
- 8. Deploy the Cloud Dataflow Pipeline

The Dataflow application uses packages and modules that don’t come as part of the standard library. To keep your Python environment isolated from your other applications it’s recommended that you use a Python virtual environment. Below are the steps to use the virtual environment for the Dataflow application.

```
virtualenv env
source env/bin/activate
# Deploy the Dataflow Pipeline
python3 saflongrunjobdataflow.py --project=[YOUR_PROJECT_ID] --
input_topic=projects/[YOUR_PROJECT_ID]/topics/[YOUR_TOPIC_NAME] --runner=DataflowRunner --
temp_location=gs://[YOUR_DATAFLOW_STAGING_BUCKET]/tmp --output_bigquery=[DATASET NAME].[TABLE] --
requirements_file="requirements.txt"
```

Replace the following parameters with your respective parameters:

```
[YOUR_PROJECT_ID] # Your Google Cloud Project ID
[YOUR_TOPIC_NAME] # Your Pub/Sub Topic Name
```

```
[YOUR_DATAFLOW_STAGING_BUCKET] # The Staging bucket created in Step 1
[DATASET NAME] # The BigQuery Dataset you created in step 3
[TABLE] # A table name of your choice, something like transcripts.
```

Dataflow will create the table for your, it has been defined to create the tables as follows in the Dataflow Pipeline code.

```
create_disposition=beam.io.BigQueryDisposition.CREATE_IF_NEEDED
```

9. Once the pipeline has successfully deployed which can take a few minutes you can continue below to test the newly added component to your cloud native application. You can confirm the pipeline installed successfully in the Cloud Console at Big Data > Dataflow. Figure 5-1 shows the successfully deployed pipeline.

![](img/eac8762c24ec77948aa0c3277f8e6eb3_145_0.png)

Figure 5-1. Cloud Dataflow Deployed Apache Beam Pipeline

For the next part, you will test the components you just deployed. In your repository you cloned, you will have a folder called sample audio files, and you will use these audio files to test the deployed components.

10. Run the following command in your terminal to process a sample stereo audio file, replace the parameters within the brackets [] with your values. The sample audio files are located
