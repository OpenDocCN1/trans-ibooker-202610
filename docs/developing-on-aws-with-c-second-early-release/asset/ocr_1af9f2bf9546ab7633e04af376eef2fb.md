Once you have the front end uploading files directly into S3 instead of sending them via a Web API, adding a trigger into the S3 bucket can be done directly in the management console on the Properties tab of the S3 bucket configuration, as shown in Figure 3-15.

![](bbox=[0.14, 0.2, 0.85, 0.78])

Figure 3-15. Adding an event notification to an S3 bucket

Add the step functions state engine as the destination for this event and we arrive at the final form of our event-driven, serverless, C# Resume Uploader Service! The final architecture is shown in Figure 3-16.