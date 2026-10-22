```
"StoredFileUrl.$": "$.StoredFileUrl"
}
},
"End": true
}
}
}
```

Now when we trigger the state machine from a file being uploaded the EmailRecruitment Lambda will not be executed, we will just get a message posted to the queue. All we need to do now is write a new AWS Lambda function to run once a day that will read all the messages from the queue and send an email containing all the file URLs. The code for such a Lambda might look like this:

```
public async Task<string> BatchEmailRecruitment(object input, ILambdaContext context)
{
    using var sqsClient = new AmazonSQSClient(RegionEndpoint.EUWest2);
    var messageResponse = await sqsClient.ReceiveMessageAsync(
        new ReceiveMessageRequest() ①
        {
            QueueUrl = queueUrl,
            MaxNumberOfMessages = 10
        });

    var stateObjects = messageResponse.Messages.Select(msg => Deserialize(msg.Body));

    var listOfFiles = string.Join("\n\n", stateObjects.Select(x => x.StoredFileUrl)); ②

    await _emailService.Send("recruitment@example.com", ③
        $"You have {messageResponse.Messages.Count} new resumes to review!\n\n"
        + listOfFiles);

    await sqsClient.DeleteMessageBatchAsync(new DeleteMessageBatchRequest() ④
    {
        QueueUrl = queueUrl,
        Entries = messageResponse.Messages.Select(x => new DeleteMessageBatchRequestEntry()
```