
Figure 10-33 shows the UI confirmation of the results of the second API call.

![](img/879ce8537ecb7481ad77093e6fcbe574_282_0.png)

## Figure 10-33. *UI Confirmation of the Results of the API Call (Second Screenshot)*

The following API call builds on the arbitrary package structure defined in the beginning of this step:

POST: https://apisalesdemo4.successfactors.com:443/odata/v2/JobApplicationBackgroundCheckResult?$format=json

Request Payload:

```json
{
  "vendorOrderNo": "MYTHICAL_12112019_87654",
  "vendorCode": "MYTHICAL",
  "stepName": "Employment",
  "stepStatus": "Submitted - pending verification",
  "stepMessage": "Background Check is in process. Please check back later",
  "finalStep": false
}
```

Response:

```
{
    "d" : {
        "__metadata" : {
            "uri" : "https://apisalesdemo4.successfactors.com:443/odata/v2/JobApplicationBackgroundCheckResult(142L)",
            "type" : "SFData.JobApplicationBackgroundCheckResult"
        },
        "vendorOrderNo" : "MYTHICAL_12112019_87654",
        "vendorCode" : "MYTHICAL",
        "stepName" : "Employment",
        "stepStatus" : "Submitted - pending verification",
        "stepMessage" : "Background Check is in process. Please check back later",
        "finalStep" : false,
        "statusId" : "142"
    }
}
```

Figure 10-34 shows the UI confirmation of the stepName called Employment.

![](img/879ce8537ecb7481ad77093e6fcbe574_283_0.png)

The following simply updates the previous API call:

POST: https://apisalesdemo4.successfactors.com:443/odata/v2/JobApplicationBackgroundCheckResult?$format=json

Request Payload:

```
{
  "vendorOrderNo": "MYTHICAL_12112019_87654",
  "vendorCode": "MYTHICAL",
  "stepName": "Employment",
  "stepStatus": "Completed - missing 2 years gap",
  "stepMessage": "Background Check is in process. Please check back later",
  "finalStep": false
}
```

Response:

```
{
  "d": {
    "__metadata": {
      "uri": "https://apisalesdemo4.successfactors.com:443/odata/v2/JobApplicationBackgroundCheckResult(143L)",
      "type": "SFOData.JobApplicationBackgroundCheckResult"
    },
    "vendorOrderNo": "MYTHICAL_12112019_87654",
    "vendorCode": "MYTHICAL",
    "stepName": "Employment",
    "stepStatus": "Completed - missing 2 years gap",
    "stepMessage": "Background Check is in process. Please check back later",
    "finalStep": false,
    "statusId": "143"
  }
}
```

Final API call to indicate that the background check is complete:

POST: https://apisalesdemo4.successfactors.com:443/odata/v2/JobApplicationBackgroundCheckResult?$format=json

## Request Payload:

```json
{
  "vendorOrderNo": "MYTHICAL_12112019_87654",
  "vendorCode": "MYTHICAL",
  "stepName": "Comprehensive Background",
  "stepStatus": "Completed - Missing information. Proceed with caution.",
  "stepMessage": "Background Check completed.",
  "reportUrl": "https://partneredge.sap.com/en/partnership/manage/mmp.html",
  "finalStep": true
}
```

## Response Payload:

```json
{
  "d": {
    "__metadata": {
      "uri": "https://apisalesdemo4.successfactors.com:443/odata/v2/JobApplicationBackgroundCheckResult(144L)",
      "type": "SFOData.JobApplicationBackgroundCheckResult"
    },
    "vendorOrderNo": "MYTHICAL_12112019_87654",
    "vendorCode": "MYTHICAL",
    "stepName": "Comprehensive Background",
    "stepStatus": "Completed - Missing information. Proceed with caution.",
    "stepMessage": "Background Check completed.",
    "reportUrl": "https://partneredge.sap.com/en/partnership/manage/mmp.html",
    "finalStep": true,
    "statusId": "144"
  }
}
```

Figure 10-35 shows the final result of the ODATA API call indicating that the background check is complete.

![](img/879ce8537ecb7481ad77093e6fcbe574_286_0.png)

Figure 10-35. Screenshot Showing the Completed Background Check with the Results URL

## Conclusion

If you have followed the steps listed earlier, you should be in a position to successfully build a background check integration for an SAP SuccessFactors Recruiting customer in two different ways. The first is when a recruiter or another authorized user initiates the action within the Job Application User Interface. The second is when a job application is moved to the Background Check status using a combination of Intelligent Services and Integration Center.

## FAQs

- 1. Can two or more background check vendors coexist in an SAP SuccessFactors Recruiting instance?
- 2. Yes, but with a caveat. Only one of the background check vendors can leverage the background check portlet. In other words, the results passed by only one vendor will be displayed on the portlet. The second vendor has to add custom fields on the job application to return results.
- 3. Can you build a background check integration without Integration Center?
- 4. If you’re going to use the background check initiated from within the job application UI, Integration Center must be used. There is no limitation if Intelligent Services are used.
- 5. Can I launch a background check at different steps in the candidate pipeline?
- 6. Yes, you can, as long as the feature permission in step 3 is repeated for each status in the applicant workflow.
- 7. What happens when a customer has more than one job requisition template or job application template?
- 8. The Integration Center templates have to accommodate for each applicable status for each template.
- 9. What if the background check vendor cannot support synchronous JSON request/response?
- 10. The Integration Center templates will not have response fields mapping. In that case, the vendor has complete control over how the JobApplicationBackgroundCheckRequest object is constructed.
- 11. What are the authentication mechanisms supported by Integration Center?
- 12. Integration Center supports the following authentication mechanisms, none, Basic Authentication or OAuth 2.0 with SAML Flow, OAuth 2.0 with Grant Type as Password, and OAuth2.0 with Grant Type as Client Credentials. Please note that Basic Authentication will be deprecated in November 2021 and fully decommissioned by December 2022.
- 13. How do I troubleshoot the integration?
- 14. If the integration is launched from the job application UI via Initiate Background Check, the starting point will be the Execution Monitor in Integration Center. There will be one execution per background check. The Execution Monitor will provide a sequential list of entries to identify the source of the problem. If the integration uses Intelligent Services, the starting point should be Intelligent Services Center ➤ Update of Job Application Status Event ➤ Event Monitor, and optionally the Execution Monitor of Integration Center (if that is employed as part of integration).

## CHAPTER 10 BACKGROUND CHECK INTEGRATION

## Candidate Offer
Automation Using
Business Rules

As we have progressed throughout the book, we’ve shown you a variety of integration and automation methods at each stage of the recruiting process. Thus far, each chapter of this book has focused on an integration or automation that involves a third-party system, creating a file, or making/receiving an external call. However, there are some automations that can be executed entirely within the SuccessFactors system. As we approach the offer stage of the recruiting process, we take this opportunity to review such automations since creating an offer is less likely to require integration (outside of standard integrations such as document signature). A prime example of these types of internal automations is business rules. Let’s take a look!

## Business Rules Overview

Business rules are a feature of SAP SuccessFactors that allow administrators to construct If/Then conditional logic and then execute a resulting scripted behavior when the conditions are met. This feature is not available for all objects and data in the system, but the list is expanding with every release. In general, business rules for scenarios where fields are initialized, changed, and saved will work on job requisitions, offers, candidates, and applications. However, there are a lot of exceptions we have covered in the following note.
