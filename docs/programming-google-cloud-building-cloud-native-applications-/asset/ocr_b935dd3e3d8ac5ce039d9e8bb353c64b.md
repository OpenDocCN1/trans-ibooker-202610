6. If you run npm install in a previous step within this folder you can skip this step: On the terminal within this folder run npm install
7. On the terminal run node createBucketLock.js
8. On successful execution, it should look like Figure 2-8

![](bbox=[0.13, 0.18, 0.86, 0.61])

Figure 2-8. Code Screenshot for createBucketLock.js

You created a new bucket with a retention lock period of 6 months. If you try to upload a new file to the bucket and delete the uploaded file you will receive an error message.

# List Buckets

Let’s now list the buckets we programmatically created. This is a useful command as it allows you to list all the buckets in your project, which can allow you to perform actions like Data Loss Prevention. We can loop through all the buckets and objects, then scan them for PII information with the Google Cloud Data Loss Prevention API. Line 12 we use a forEach() which calls the provided callback function once for each element in an array. Each element in the array is our list of buckets within our project.

```
// Imports the Google Cloud client library
const {Storage} = require('@google-cloud/storage');
// Creates a client
const storage = new Storage({
keyFilename: "key.json",
projectId: "ruicosta-blog"
});
```