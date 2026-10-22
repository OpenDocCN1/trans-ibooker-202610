# C# Resume Uploader Example: SQS

Think about what we can do with our Serverless C# Resume Uploader application to take advantage of SQS or SNS. We know users can upload their resume to our API (via API Gateway) which will store it in S3 and then use Textract to read the candidate’s GitHub profile. How about instead of immediately then emailing our recruitment team we add a message to a queue? That way we can run a job once every morning and send *one* email for *all* the messages that have built up on the queue during the past 24 hours. This might make it easier for our recruitment team to sit down and read all of the day’s resumes at once instead of doing it piecemeal throughout the day. Can we do all of this without having to touch the rest of our code? Without having to rebuild, re-test or redeploy the entire application? Yes, and here’s how.

Since we are using AWS Step Functions we can simply create a task in our definition JSON file that posts a message to an SQS queue. We don’t need another Lambda function to do this, instead we can specify the message body directly in the JSON and then update our state engine definition. Here we have the task called "QueueEmail" that posts a message to an SQS queue we have set up called UploadedResumeFiles, referenced here by its ARN.

```
{
  "Comment": "Resume Uploader State Machine",
  "StartAt": "SaveUploadedResume",
  "States": {
    "SaveUploadedResume": {
      "Type": "Task",
      "Resource": "arn:aws:lambda:eu-west-2:00000000:function:UploadNewResume",
      "Next": "QueueEmail"
    },
    "QueueEmail": {
      "Type": "Task",
      "Resource": "arn:aws:states:::sqs:sendMessage",
      "Parameters": {
        "QueueUrl": "https://sqs.eu-west-2.amazonaws.com/00000000/UploadedResumeFiles",
        "MessageBody": {

```