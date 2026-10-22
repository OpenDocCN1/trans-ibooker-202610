
As discussed, once the file is written to Cloud Storage it will trigger a Cloud Function. Below is a code snippet of the cloud function we will deploy.

```
let pubSubObj = {
  'fileid': uniqid.time(),
  'filename': `gs://${file.bucket}/${file.name}`,
  'callid': file.metadata.callid === undefined ? 'undefined' : file.metadata.callid,
  'date': Date(Date.now()),
  'year': file.metadata.year === undefined ? 'undefined' : file.metadata.year,
  'month': file.metadata.month === undefined ? 'undefined' : file.metadata.month,
  'day': file.metadata.day === undefined ? 'undefined' : file.metadata.day,
  'starttime': file.metadata.starttime === undefined ? 'undefined' :
    file.metadata.starttime,
  'duration': duration, //get value from fluent-ffmpeg
  'stereo': file.metadata.stereo === undefined ? 'undefined' : file.metadata.stereo,
};
```

This Javascript object will hold the key:value pairs from our custom metadata. Notice we are not referencing x-goog-meta when extracting the value. Again, we ignore the prefix even within our codebase.

Now that we have a good understanding of Cloud Storage as well as how the framework works with Cloud Storage, let's move on to Cloud Functions. Cloud Functions plays an important role in our framework as it will be the first step to start the process of transcribing the audio files.

## Cloud Functions

Google Cloud Functions is a serverless compute solution that allows you to run event-based applications. Cloud Functions allows you to write single-purpose functions which can be attached to events from your services. An example is how our framework will leverage cloud functions. Our function will be triggered by an event, and the event will trigger a Cloud Function once an object is written to the Cloud Storage bucket that the function is attached to.

Cloud Functions is a serverless offering from Google Cloud. It abstracts the compute infrastructure and allows you to focus on your code. You don't have to worry about patching operating systems or provisioning resources. Cloud Functions scale automatically; they can scale from a single invocation to millions without intervention from the developer. For Cloud Storage, Cloud Functions supports the following trigger types:

- google.storage.object.finalize
- google.storage.object.delete
- google.storage.object.archive
- google.storage.object.metadataUpdate

Our framework will leverage the google.storage.object.finalize trigger type. This event is triggered when a new object is created or an existing object is overwritten in the bucket. Below is a code snippet from our Cloud Function.

```
exports.safLongRunJobFunc = (event, context, callback) => {
  const file = event;
  const topicName = file.metadata.pubsubtopicname;
  const audioPath = { uri: `gs://${file.bucket}/${file.name}` };
  const readFile = storage.bucket(file.bucket).file(file.name);
```

On line 1, the event parameter in the first argument is the storage event data that is delivered in the Cloud Storage object format. When our event is triggered Cloud Storage will expose this object that contains information about our object including the custom metadata you applied when you uploaded the file via the gsutil command. Below is the object values that are exposed to the cloud function:

```
{
  "kind": "storage#object",
  "id": string,
  "selfLink": string,
  "name": string,
  "bucket": string,
  "generation": long,
  "metageneration": long,
  "contentType": string,
  "timeCreated": datetime,
  "updated": datetime,
  "timeDeleted": datetime,
  "temporaryHold": boolean,
  "eventBasedHold": boolean,
  "retentionExpirationTime": datetime,
  "storageClass": string,
  "timeStorageClassUpdated": datetime,
  "size": unsigned long,
  "md5Hash": string,
  "mediaLink": string,
  "contentEncoding": string,
  "contentDisposition": string,
  "contentLanguage": string,
  "cacheControl": string,
  "metadata": {
    (key): string
  },
  "acl": [
    objectAccessControls Resource
  ],
  "owner": {
    "entity": string,
    "entityId": string
  },
  "crc32c": string,
  "componentCount": integer,
  "etag": string,
  "customerEncryption": {
    "encryptionAlgorithm": string,
    "keySha256": string
  },
  "kmsKeyName": string
}
```

From our code snippet, we extract the following values:

```
const topicName = file.metadata.pubsubtopicname;
const audioPath = { uri: `gs://${file.bucket}/${file.name}` };
const readFile = storage.bucket(file.bucket).file(file.name);
```

The topicName is a custom metadata that we applied on upload, -h x-goog-meta-pubsubtopicname: [TOPIC_NAME] and now we have access to it in our code.

## Cloud Function Libraries

Cloud Functions provides you the ability to use external modules. The dependencies for our framework are managed with npm and defined in a file called package.json. This allows developers to be creative with their functions. Our framework leverages the following external modules:

```
const PubSub = require(`@google-cloud/pubsub`);
const storage = require('@google-cloud/storage')();
const speech = require('@google-cloud/speech').v1p1beta1;
const client = new speech.SpeechClient();
const uniqid = require('uniqid');
const ffmpeg = require('fluent-ffmpeg');
const ffmpegPath = require('@ffmpeg-installer/ffmpeg').path;
```

The fluent-ffmpeg and ffmpeg-installer/ffmpeg allow our function to check the codec and length of the audio file. When submitting a job to Speech-to-Text for transcription we need to configure the sampleRateHertz parameter. Using the external libraries, we extract this information and pass it to the Speech-to-Text API. We are using the long-running job method available from the Speech-to-Text API. When we submit an audio file to be transcribed for a long-running job we need to wait for the API to complete, this process can take minutes to hours. The formula used is n/2 for how long the API will take to complete. For this we need to wait a certain amount of time before we check to see if the API is done.

If we continuously poll the API we might hit a quota limit with Google Cloud. To avoid hitting a quota we only check the API for a completed job after the duration of the audio file divided by two is completed. Example: If the audio file is 1 hour long we will only check the API for completion after 30 minutes have passed. We will cover this in-depth in the Dataflow chapter as here we only submit the job to Speech-to-Text and receive the job name which then is sent as a key:value pair to Pub/Sub.

## GitLab CI/CD and Cloud Functions

Let's practice deploying a Cloud Function with GitLab CI/CD.

> NOTE
> You can practice along now on your local workstation or at a later time using Qwiklabs. The Qwiklabs Quest for this book is coming available soon.

We leverage a core component of Cloud-native applications Continuous Integration (CI) and Continuous Delivery (CD) architecture to automate deployment tasks, and perform peer reviews prior to pushing the code to production. For our example we will leverage GitLab for Source Code Management and CI/CD.

For our first integration step with GitLab https://about.gitlab.com/, we will keep it simple and focus on getting our code developed and deployed faster. You will need a GitLab account to follow along. To authorize GitLab access to our Google Cloud project we will need to create a service account and assign the required roles. Always keeping in mind to work with least privilege methodology.

- 1. Open the IAM & Admin in the Google Cloud Console:

![](img/eac8762c24ec77948aa0c3277f8e6eb3_64_0.png)

- 2. On the menu click Service Accounts:

![](img/eac8762c24ec77948aa0c3277f8e6eb3_64_1.png)

- 3. Click CREATE SERVICE ACCOUNT:

- 4. Enter your Service account details:
  a. Service Account Name
  b. Service Account ID
  c. Service Account Description
- 5. Click **Create**.
- 6. Assign the following roles to the Service Account
  a. Cloud Functions Admin
  b. Service Account User
- 7. Click Continue
- 8. Click Done
- 9. Locate the newly created Service Account, click on the actions icon and select create key:
- 10. Choose JSON and click CREATE. This will download the JSON file to your local workstation.
- 11. Head over to your GitLab Project, and under settings go to CI/CD
- 12. Click Expand Variables
- 13. Create two new variables labeled as:
  a. PROJECT_ID
  b. SERVICE_ACCOUNT
- 14. Enter your Google Cloud project id
- 15. Open the JSON file for the service account you downloaded before, copy and paste its content to the SERVICE_ACCOUNT key. It should look something like Figure 2-12:
