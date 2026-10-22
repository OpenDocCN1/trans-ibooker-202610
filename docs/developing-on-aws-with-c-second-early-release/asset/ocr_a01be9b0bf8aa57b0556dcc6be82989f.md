When this state input is passed to the first function, UploadNewResume (), it will have the state.FileBase64 set by API Gateway from the HTTP POST request from our front end. The function will save this file to S3 then set state.StoredFileUrl before passing the state object to the next function. It will also clear state.FileBase64 from the state object. The state object in AWS Step Functions is passed around between each step and since this base 64 string will be quite large we can set it to null after reading it to reduce the size of the state object that is passed on.

We can deploy these two functions to AWS Lambda as we did before, however this time we need to specify the --function-handler parameter for each. The function handler is the C# function in our code that acts as the entry point to our Lambda, as we saw previously in “Developing with AWS Lambda and C#”.

```
dotnet lambda deploy-function UploadNewResume
  --function-handler
  ServerlessResumeUploader::ServerlessResumeUploader.
    LambdaFunctions::UploadNewResume
```

```
dotnet lambda deploy-function EmailRecruitment
  --function-handler
  ServerlessResumeUploader::ServerlessResumeUploader.
    LambdaFunctions::EmailRecruitment
```

Now if we go into AWS Step Functions we can create a new state machine, connect these two Lambdas together and to API Gateway. The AWS Management Console does give us a graphical user interface to build up these workflows, however, we are engineers so we are going to write it in Amazon States Language in a JSON file. This also has the crucial benefit of utilizing a plaintext JSON file that we can check into source control, giving us a great Infrastructure as Code (IaC) deployment model. The JSON configuration for our simple workflow here looks like this:

```
{
  "Comment": "Resume Uploader State Machine",
```