Response payload:

```
{
  "d": [
    {
      "key": "JobApplicationAssessmentReport/id=212",
      "status": "OK",
      "editStatus": null,
      "message": "Assessment Report has been inserted successfully",
      "index": 0,
      "httpCode": 200,
      "inlineResults": null
    }
  ]
}
```

Notes:

+   1. The orderId field is the primary key in the JobApplicationAssessmentOrder shown in the earlier query.
+   2. The status field can take the following values:
+      a. 2 (INITIATED – shown as Initiated in the UI)
+      b. 1 (IN_PROGRESS – shown as Pending in the UI)
+      c. 0 (COMPLETED – shown as Completed in the UI)
+      d. 99 (INITIATION_FAILED – shown as Error ordering assessment in the UI)
+   3. statusDate is always passed in Unix epoch time. In the earlier example, 1566422180219 means 8/21/2019, 2:16:20 PM Pacific Daylight Time. Current Milliseconds is an excellent resource for this.

The result can be seen in the Assessments portlet on the candidate profile as shown in Figure 8-21