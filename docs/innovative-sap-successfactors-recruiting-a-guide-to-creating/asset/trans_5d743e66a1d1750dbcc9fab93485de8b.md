

![](img/879ce8537ecb7481ad77093e6fcbe574_261_0.png)

![](img/879ce8537ecb7481ad77093e6fcbe574_261_1.png)

> *Figure 10-16.* Sample Completed Job Requisition with Background Check Fields Chosen

## Step 11: Recruiter Actions – Initiate Background Check

Login as the recruiter or proxy and navigate to Recruiting ➤ Job Requisitions. Select the job requisition you want to work on and move (or advance) the candidate to the Background Check status. The option to move or advance is dependent on the customer configuration. A note on the screenshots is shown – they are taken from different job requisitions and candidate, but the concept holds regardless of which candidate and job requisition combination you work with.

Figure 10-17 shows a sample job requisition with two candidates who are in the Background Check status.

![](img/879ce8537ecb7481ad77093e6fcbe574_262_0.png)

Figure 10-17. Job Requisition with Two Candidates in the Background Check Status

Click a desired candidate and go to Take Action ▶ Initiate Background Check as shown in Figure 10-18.

![](img/879ce8537ecb7481ad77093e6fcbe574_262_1.png)

Figure 10-18. Screenshot Showing How to Initiate a Background Check

If the background check submission is successful, you should see a screenshot as in Figure 10-19.

![](img/879ce8537ecb7481ad77093e6fcbe574_263_0.png)

Figure 10-19. Sample Screenshot Showing Successful Initiation of a Background Check

If you have made it this far, congratulations. You have successfully defined MYTHICAL as a background check vendor and have been able to initiate a background check from the Recruiting Job Application UI, namely, Option 1. We will examine Option 2 (IS) shortly.

But what happens if there is an error showing that your background check failed?

## Step 12: Integration Center – Checking Results of Outbound Execution

Navigate to Admin Center ► Integration Center and click Monitor Integrations as shown in Figure 10-20.

### Figure 10-20. Integration Center Highlighting the Integration Monitor

The Execution Manager screen will open in a new pop-up where you can see the results of current and past executions. By default, the Execution Manager will show the executions in the last seven days. The filters can be adjusted depending on the time filter and optional the Process Status. An example is shown in Figure 10-21.

### Figure 10-21. Screenshot of the Execution Manager Showing Summary of Executions in the Last Seven Days

![](img/879ce8537ecb7481ad77093e6fcbe574_264_0.png)

![](img/879ce8537ecb7481ad77093e6fcbe574_264_1.png)

Figure 10-22 shows the drill-down details of the first execution shown in Figure 10-21. To get the details, click the arrow and view the screen.

![](img/879ce8537ecb7481ad77093e6fcbe574_265_0.png)

*Figure 10-22. Illustration of a Successful Background Check Request*

Detailed documentation on the Execution Manager is available on the standard Help Portal.

- **Notes:**
  - 1. This should be the starting point for troubleshooting any integrations.
  - 2. Customers will often use the download link to export a CSV file for support issues.
  - 3. You (or the customer) can optionally choose to notify an email address or an inbox by clicking Manage Notifications.
  - 4. If the customer is initiating background checks in the user interface, you should see only one record per background check.

## Step 13: Setting Up the Integration Using Intelligent Services Center (Option 2)

In the beginning of this chapter, we described two ways to integrate background checks, namely, via the UI (Option 1) and via Intelligent Services (Option 2). Chapter 12 of this book goes into some detail on how to use Intelligent Services. In this step, we will highlight how to set up the integration. Figure 10-23 shows a screenshot with the Update of Job Application Status highlighted.

![](img/879ce8537ecb7481ad77093e6fcbe574_266_0.png)

### Figure 10-23. Intelligent Services Center with Update of Job Application Status Highlighted

Figure 10-24 shows where to create a new integration. In the beginning of this chapter, we mentioned that the underlying starting entity for the background check is JobApplication. You can associate an existing Integration Center template (ICD) or create a new one – you will be prompted by the system. You may refer to Chapter 7 on the Integration Center to create your integration or use an existing one.

![](img/879ce8537ecb7481ad77093e6fcbe574_266_1.png)

### Figure 10-24. Screen Showing How to Add a New Integration to an Event

Regardless of whether you create a new ICD or use an existing one, please be sure to click Save Flow as shown in Figure 10-25. If you don’t save your flow, your background check will be initiated and the event will be raised, but your integration will not fire and consequently never reach the endpoint.

## Step 16: Intelligent Services Center – Checking the Results of Outbound Execution

If your integration is all set up, the next logical step is to change the status of the job application to Background Check and examine the results of your integration. The first step is to examine if an event associated with the status change has been raised. Figure 10-28 shows such an example showing that the Update of Job Application Status has indeed been raised and there are two possible events to examine. Click the number of executions in the right column.

![](img/879ce8537ecb7481ad77093e6fcbe574_269_0.png)

Figure 10-28. Intelligent Services Center Showing Number of Events Raised Against Events

The highlighted items show the two events for Update of Job Application Status Intelligent Services event. Click the number 1 to view the execution status as shown in Figure 10-29. There will be one event per update of job application status.

![](img/879ce8537ecb7481ad77093e6fcbe574_271_0.png)

Figure 10-31. Sample Illustration of the Execution Manager for the Raised Event for Background Check

If you have gotten this far, your outbound integration for the background check has been set up, and it is now time to perform the API operations so that customers can receive the results from the background check vendor and view them in the recruiting user interface. Please refer to Chapter 1 on how to use and leverage ODATA APIs.

## Step 17: Query JobApplicationBackgroundCheckRequest for Option 1

If the outbound Integration Center is set up successfully and the background vendor is able to receive the request and respond synchronously, the JobApplicationBackgroundCheckRequest should be completely populated:

```
GET: https://apisalessdemo4.successfactors.com:443/odata/v2/JobApplicationBackgroundCheckRequest?$format=json&$filter=requestId eq 140
```

RESPONSE:

```
```
{
    "d": {
        "results": [
            {
                "__metadata": {
                    "uri": "https://apisalessdemo4.successfactors.com:443/odata/v2/JobApplicationBackgroundCheckRequest(140L)",
```
```

```
```json
"type": "SFOD ata.JobApplicationBackgroundCheckRequest"
},
"requestId": "140",
"vendorOrderNo": "MYTHICAL_12112019_87654",
"lastModifiedDateTime": "/Date(1576094482000+0000)/",
"createdByUser": "sfadmin",
"createdDateTime": "/Date(1575936725000+0000)/",
"orderStatus": "OK",
"applicationId": "3040",
"responseDetail": "Order received and acknowledged.",
"responseCode": "200",
"vendorCode": "MYTHICAL",
"jobApplication": {
    "__deferred": {
        "uri": "https://apisalesdemo4.successfactors.com:443/odata/v2/JobApplicationBackgroundCheckRequest(140L)/JobApplication"
    }
}
}
]
}
```
```

### Notes:

- 1. requestId is the SuccessFactors-generated primary key for the JobApplicationBackgroundCheckRequest entity and cannot be changed. There will be one or more requests per background request.
- 2. vendorOrderId is the value set by the vendor as part of the synchronous response. You will use this vendorOrderId to send subsequent results.
- 3. vendorCode is set during the outbound Integration Center set up in Manage Data. Please refer to step 8 of this chapter.

