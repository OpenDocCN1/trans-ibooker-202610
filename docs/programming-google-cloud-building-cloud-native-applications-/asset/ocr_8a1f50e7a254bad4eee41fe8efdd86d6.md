# Figure 1-5. Cloud Dataflow Pipeline

Cloud Dataflow is a powerful tool to perform ETL operations on our data (Cloud Dataflow is covered in more detail in Chapter XYZ]. Cloud Dataflow is a core component of the framework as it pulls the messages from Pub/Sub enriches the data by calling additional APIs within the pipeline and finally writes the data to BigQuery allowing us to visualize the metrics collected from the audio file.

Cloud Dataflow pipelines can be written in Python or Java. For our framework, we have chosen Python as it is easier to read and understand even if you don’t have a programming background. As an example, review the code snippet below and try to determine what those lines of code are doing.

```
def stt_output_response(data):
    from oauth2client.client import GoogleCredentials
    from googleapiclient import discovery
    credentials =
GoogleCredentials.get_application_default()
    pub_sub_data = json.loads(data)
    speech_service = discovery.build('speech',
'v1p1beta1', credentials=credentials)
    get_operation = speech_service.operations().get(
                    name=pub_sub_data['sttnameid'])
    response = get_operation.execute()
    # handle polling of STT
    if pub_sub_data['duration'] != 'NA':
        sleep_duration =
round(int(float(pub_sub_data['duration'])) / 2)
    else:
        sleep_duration = 5
    logging.info('Sleeping for: %s', sleep_duration)
    time.sleep(sleep_duration)
    retry_count = 10
    while retry_count > 0 and not response.get('done',
False):
        retry_count -= 1
        time.sleep(120)
        response = get_operation.execute()
```