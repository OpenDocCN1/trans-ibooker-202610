```
"inlineResults": null
}
]
}
```

# Notes:

+   1. The orderId field is the primary key in the JobApplicationAssessmentOrder shown in the earlier query.
+   2. The status field can take the following values:
+      a. 2 (INITIATED – shown as Initiated in the UI)
+      b. 1 (IN_PROGRESS – shown as Pending in the UI)
+      c. 0 (COMPLETED – shown as Completed in the UI)
+      d. 99 (INITIATION_FAILED – shown as Error ordering assessment in the UI)
+   3. statusDate is always passed in Unix epoch time. In the earlier example, 1566423670260 means 8/21/2019, 2:41:10 PM Pacific Daylight Time.
+   4. The [ and ] characters indicate that the details are passed as an array to the assessmentReportDetail navigable entity on the JobApplicationAssessmentOrderReport entity. The underlying ODATA entity is JobApplicationAssessmentReportDetail.
+   5. You may not directly upsert to JobApplicationAssessmentReportDetail entity. You will see an error response as follows:

```
{
  "d": [
    {
      "key": null,
      "status": "ERROR",
      "editStatus": null,
      "message": "Entity JobApplicationAssessmentReportDetail is not editable. Please check the entity setting in Admin Center > OData API Data Dictionary or the entity metadata.",
    }
  ]
}
```