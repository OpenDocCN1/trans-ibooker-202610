
# StateMachineExecution0001

Edit state machine

New execution

Stop execution

Details

Execution input

Execution output

Definition

Execution Status

Succeeded

Started

Feb 18, 2022 06:01:00.070 PM

End Time

Feb 18, 2022 06:01:00.410 PM

## Execution event history

| ID | Type | Step | Resource | Elapsed Time (ms) | Timestamp |
|---|---|---|---|---|---|
| 1 | ExecutionStarted |  | - | 0 | Feb 18, 2022 06:01:00.070 PM |
| 2 | TaskStateEntered | UploadResume | - | 71 | Feb 18, 2022 06:01:00.141 PM |
| 3 | LambdaFunctionScheduled | UploadResume | Lambda | CloudWatch Logs | 71 | Feb 18, 2022 06:01:00.141 PM |
| 4 | LambdaFunctionStarted | UploadResume | Lambda | CloudWatch Logs | 119 | Feb 18, 2022 06:01:00.189 PM |
| 5 | LambdaFunctionSucceeded | UploadResume | Lambda | CloudWatch Logs | 278 | Feb 18, 2022 06:01:00.348 PM |
| 6 | TaskStateExited | UploadResume | - | 278 | Feb 18, 2022 06:01:00.348 PM |
| 7 | TaskStateEntered | QueueEmail | - | 289 | Feb 18, 2022 06:01:00.359 PM |
| 8 | TaskScheduled | QueueEmail | - | 289 | Feb 18, 2022 06:01:00.359 PM |
| 9 | TaskStarted | QueueEmail | - | 300 | Feb 18, 2022 06:01:00.370 PM |
| 10 | TaskSucceeded | QueueEmail | - | 340 | Feb 18, 2022 06:01:00.410 PM |
| 11 | TaskStateExited | QueueEmail | - | 340 | Feb 18, 2022 06:01:00.410 PM |
| 12 | ExecutionSucceeded |  | - | 340 | Feb 18, 2022 06:01:00.410 PM |

Figure 3-7. Step Functions execution log in the management console

# C# Resume Uploader Example: Step Functions

To demonstrate what we can do with AWS Step Functions, let’s take the AWS Lambda function we created in the previous part of our Resume Uploader example and split it out from one, to multiple Lambda functions. The UploadNewResume Lambda function can have the single responsibility of uploading the file to S3, then we have a second Lambda to send the email (EmailRecruitment). The HTTP part can also be abstracted away entirely and handled by the API Gateway. This gives us much more flexibility to change or optimize our workflow on the back end even after these two functions have been developed, tested and deployed

```
[assembly:
LambdaSerializer(typeof(Amazon.Lambda.Serialization.SystemTextJson.
    DefaultLambdaJsonSerializer))]

namespace ServerlessResumeUploader
{
    public class LambdaFunctions
    {
        private readonly IStorageService _storageService = new AwsS3StorageService();
        private readonly IEmailService _emailService = new AwsSesEmailService();

        ①
        public async Task<StepFunctionsState>
UploadNewResume(StepFunctionsState state,

ILambdaContext context)
        {
            byte[] bytes =
Convert.FromBase64String(state.FileBase64);
            using var memoryStream = new MemoryStream(bytes);

            state.StoredFileUrl = await
_storageService.Upload(memoryStream);

            state.FileBase64 = null;
```

```
return state;
}
public async Task<StepFunctionsState>
EmailRecruitment(StepFunctionsState state,
ILambdaContext context)
{
    await _emailService.Send("recruitment@example.com",
        $"Somebody uploaded a resume! Read it here:
{state.StoredFileUrl}\n\n" +
        $"...and check out their Github profile:
{state.GithubProfileUrl}");

    return state;
}
}
```

The first Lambda function that takes a file and saves to S3.
The second Lambda that emails our recruitment team a link to the file.

You can see the code for this is very similar to the ApplicationController code we had earlier, in this chapter except it has been refactored into two methods on a LambdaFunctions class.
This will be deployed as two individual AWS Lambda functions that use the same binaries, a fairly common way to deploy multiple Lambda functions that share code. The actual uploading and email sending is again performed by the AwsS3StorageService and AwsSesEmailService, however we are not using dependency injection any more⁵. Also note that for these two functions the first parameter is now state and they are both of the type StepFunctionsState. AWS Step Functions executes your workflow by passing a state object between each task (in this case a task is a function). The functions add to or remove properties from the state. The state for our functions above looks like this:

```
public class StepFunctionsState
{
    public string FileBase64 { get; set; }

    public string StoredFileUrl { get; set; }
}
```

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

```
"StartAt": "SaveUploadedResume",
"States": {
    "SaveUploadedResume": {
        "Type": "Task",
        "Resource": "arn:aws:lambda:eu-west-2:00000000:function:UploadNewResume",
        "Next": "EmailRecruitment"
    },
    "EmailRecruitment": {
        "Type": "Task",
        "Resource": "arn:aws:lambda:eu-west-2:00000000:function:EmailRecruitment",
        "End": true
    }
}
```

The ARN (Amazon Resource Name) of the Lambda function can be found in the AWS Lambda section of the Management Console and it allows us to reference these Lambda functions from anywhere in AWS. If we copy the above JSON into the definition of our AWS Step Functions state machine you can see in the console that it creates a graphical representation of the workflow for us. Figure 3-11 later in this chapter shows what that looks like.

The last step in this example is to connect the start of our state machine to API Gateway. Whereas previously API Gateway was configured to proxy all requests to our AWS Lambda function, we now want to specify the exact route in API Gateway and forward it to our step functions workflow. Figure 3-8 shows the setup of a POST endpoint called /applications that has “Step Functions” set as the AWS Service. You can also see in Figure 3-8 that we have set Content Handling to “Convert to text (if needed)”. This is an option specific for our resume uploader example here. Since we will be POSTing the PDF file to our API as raw binary, this option tells API Gateway to convert that to Base64 text for us. This can then be easily added onto our state object (represented by StepFunctionsState.cs shown earlier) and passed around between Lambda functions on our AWS Step Functions workflow.

# /application - POST - Setup

Choose the integration point for your new method.

Integration type
Lambda Function
HTTP
Mock
AWS Service
VPC Link

AWS Region
eu-west-2

AWS Service
Step Functions

AWS Subdomain

HTTP method
POST

Action Type
Use action name
Use path override

Action
StartExecution

Execution role
arn:aws:iam::XXXXXXXXXX:role/MyLambdaRole

Content Handling
Convert to text (if needed)

Use Default Timeout

Save

Figure 3-8. API Gateway configuration for triggering Step Functions

Lastly, we can set up some request mapping in API Gateway to transform the request into a JSON object that tells AWS Step Functions which state
