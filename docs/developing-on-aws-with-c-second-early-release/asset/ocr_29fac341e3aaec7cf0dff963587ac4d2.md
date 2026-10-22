relatively simple to hook up an email forwarder that will extract an attached PDF file and save it to S3. This would then trigger the same workflow on the back end, because we have hooked our logic up to the S3 event and not some intermediary HTTP API call. The statement “when we see a new PDF file in S3, kick off our workflow” becomes much more natural to the business problem we are solving. It doesn’t actually matter any more how the PDF file gets in there our system reacts in the same way.

As far as uploading the PDF file directly to S3 goes we have options. If we already happen to be using AWS Amplify⁷ on the front end then we can use the Storage module and the "protected" upload method, restricting access to a path in our S3 bucket based on the Cognito Identity of the authenticated user:

```
amplify add storage
```

The frontend JavaScript used to upload a file to S3 would look like this:

```
import Amplify, { Auth, Storage } from 'aws-amplify';

Amplify.configure({ Storage: { AWSS3: {
    bucket: '<bucket-name>'
  } } });

async function uploadFile(fileName, file) {
  await Storage.put(fileName, file, { level: 'protected',
    contentType: file.type });
}
```

Even without AWS Amplify and an authenticated user on the front end, there is still a way we can upload directly to the S3 bucket using a Pre-Signed URL. The process for this is to create a Lambda function behind an API gateway that, when executed, will call AmazonS3Client.GetPreSignedURL() and return that to the front end to use to upload the file. Sure, you still have an API in this scenario, but the function of this API is much more in line with performing one generic task. You could, after all, use the presigned URL to upload other types of file in the front end and hook multiple step function workflows to each.