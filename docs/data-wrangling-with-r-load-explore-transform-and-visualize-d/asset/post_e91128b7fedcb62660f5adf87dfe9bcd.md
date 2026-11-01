
```
    "OutputLocation": "s3://<<path to store athena results>>/"
  }
}
```

**Wait for task to complete**: Check the checkbox. This is very important to do so that the workflow waits for the completion of the query. If you fail to check this checkbox, the subsequent steps will fail when they try to retrieve the results of an Athena query that has not finished yet.

## Step 3: Consolidate the results from the previous step using a Lambda function.

### Step 3a: We will add a “pass step” here to get the results from the map function and pass it to the Lambda function.

Search for “Pass” step and drag it into our workflow after the map function. Click on **Pass step**, navigate to the **Input** tab, then choose the **Transform input with Parameters** checkbox and enter the following values:

```
{
  "Resultpath.$": "$[*].QueryExecution.ResultConfiguration.OutputLocation",
  "query.$": "$[*].QueryExecution.Query"
}
```

### Step 3b: Create a Lambda function that will consolidate the results from the Athena query execution.

Open the Lambda console in another tab and enter the function name as `Consolidateathena results` and runtime as "Python3.9". Leave the rest of the arguments at their defaults and create the Lambda function.

### Step 3c: Update the following code in the Lambda function. Click Deploy to save the Lambda code.

The following code reads data from the S3 location and consolidates it to be sent through an email:

```
import json
import boto3
import csv
from urllib.parse import urlparse
s3 = boto3.resource('s3')
def Lambda_handler(event, context):
    # TODO implement
    i=0
    output=""
    for file in event["Resultpath"]:
        #Get a S3 URL from which we read the results of Athena query.
        parsed_path = urlparse(file, allow_fragments=False)
        #Read data from the S3 file and get results and split each line
        s3_object = s3.Object(parsed_path.netloc,
        parsed_path.path.lstrip('/'))
        data = s3_object.get()['Body'].read().decode('utf-8').splitlines()
        #Create a new section for each query output and print the output from those
        queries
            output=output+"-------------------------------\n" + event["query"][i] +"\n "
            i=i+1
            lines = csv.reader(data)
            headers = next(lines)
            output=output + "Header -"+ ",".join(headers)+"\n"
            for line in lines:
                output=output+",".join(line)+"\n "
    return output
```

### Step 3d: Update the IAM role attached to the Lambda function to have Amazon S3 permissions. We will attach an `AmazonS3FullAccess` managed policy with the IAM role. In a production setup, provide access only to the needed S3 location to further restrict access.

### Step 3e: Search for `Lambda` in the step function console and pull that into our workflow after the pass step. Enter the function name parameter with the Lambda function that was created in *step 3b*, as illustrated here:

![](img/ec1933a597ce63afa3bcf7496364f412_257_0.png)

Figure 7.49: Step function workflow – Lambda function to consolidate the results from the Athena query execution

### Step 3f: Filter the output path to fetch only the Lambda output to be sent in an email through an SNS topic. In the **Output** tab, choose **Filter output with OutputPath** and enter `$.payload`.

## Step 4: Send the consolidated results through email.

### Step 4a: Navigate to the SNS console in a separate tab and create an SNS topic with a **Standard** topic type. Once the topic is created, click on the topic, and create a subscription from the topic. Select **Email** as the protocol and enter your email address (or a group) in the **Endpoint** field. Ensure that you click on the confirm subscription email.

### Step 4b: Move back to the step function workflow and search for `sns publish`. Drag the **SNS Publish API** icon to the workflow after the Lambda function.

We have now completed creating our workflow. Click on **Next** and verify that all the components and configurations are provided without any mistakes. This is how it should look:

Start

Query

Athena StartQueryExecution

Pass

Lambda Invoke

SNS Publish

End

Figure 7.50: Step function workflow – Overall workflow for serverless data quality pipeline

## Step 5: Enter a name for the step function workflow and leave the rest of the arguments at their defaults and create the state machine. We will verify if the IAM role attached to the state machine has all the required access. You can access the IAM role for the step machine by clicking on the state machine name and there will be a link to the IAM role under the Details section.

You might have noticed a warning while creating the step functions about DynamoDB permissions being not added to the IAM role attached to the state machine. We will add the AmazonDynamoDBReadOnlyAccess managed policy to the IAM role.

Now, execute the step function with default arguments and wait for its completion. If all the configurations and IAM permissions are provided correctly, you can see the state machine getting completed successfully, as follows:

![](img/ec1933a597ce63afa3bcf7496364f412_259_0.png)

Figure 7.51: Step function workflow – Successful execution of the data quality pipeline

You can also click on the current execution and explore the input and output values from each stage to understand the workings of the state machine:

![](img/ec1933a597ce63afa3bcf7496364f412_259_1.png)

Figure 7.52: Step function workflow – Verifying the execution to understand its workings

You can then check your email to verify whether an email with consolidated results has been delivered to you. This is what it should look like:

## AWS Notification Message

![](img/ec1933a597ce63afa3bcf7496364f412_260_0.png)

Figure 7.53: Snippet from the email received after the successful completion of the data quality workflow

We now have a working serverless data quality pipeline that is customizable. We can change the existing data quality queries, add more data quality queries, and add a new application with its own set of data quality queries. But the data quality pipeline will execute them without any changes.

## CHALLENGE EXERCISE

Can you change the email output to have a summary table with rules executed and their pass/fail status along with sample records only from failed rules for further analysis?

### HINT

Design data quality queries such that they output records that don’t fit the rule. So, if there is no recorded output from the query, the data quality rule should be considered passed and vice versa. You can add this logic in the Lambda function. Collect the results of each query as you iterate through, and finally print the summary dashboard. Also, append sample records to output data in the email only when they are in failed status.

## Automating the data quality pipeline

The serverless pipeline is currently executed in an ad hoc manner. We can make the pipeline execute on a custom schedule through EventBridge Scheduler.

Navigate to the EventBridge console and choose **Schedules**, as shown here:

![](img/ec1933a597ce63afa3bcf7496364f412_261_0.png)

Figure 7.54: EventBridge Scheduler from the AWS console

Choose **Create schedules** and provide a name for the schedule. We will currently schedule the data quality pipeline to be executed every hour using a recurring schedule, but you can set up more complex schedules using a cron-based approach as well, as seen here:

## Schedule pattern

Occurrence

Info

You can define an one-time or recurrent schedule.

One-time schedule

Recurring schedule

Schedule type

Choose the schedule type that best meets your needs.

Cron-based schedule

A schedule set using a cron expression that runs at a specific time, such as 8:00 a.m. PST on the first Monday of every month.

Rate-based schedule

A schedule that runs at a regular rate, such as every 10 minutes.

Rate expression

Info

Enter a value and the unit of time to run the schedule.

rate ( 1 hours )

Flexible time window

If you choose a flexible time window, Scheduler invokes your schedule within the time window you specify. For example, if you choose 15 minutes, your schedule runs within 15 minutes after the schedule start time.

Off

![](img/ec1933a597ce63afa3bcf7496364f412_262_0.png)

Figure 7.55: EventBridge Scheduler – Setting up a schedule to run every hour

Under Target API, choose the AWS Lambda (Invoke) option and select the step function that we created in the last section, as shown here:

![](img/ec1933a597ce63afa3bcf7496364f412_262_1.png)

# StartExecution

AWS Step Functions

State machine

athenaserverlessexec

Create new state machine

Figure 7.56: EventBridge Scheduler – Calling the step function as the target

Leave the rest of the arguments at their defaults and create the schedule. Congrats! We have now set up a serverless data quality pipeline that is executed on a schedule.

You can see from the following screenshot that the workflow has been running every hour and executing the data quality queries:
