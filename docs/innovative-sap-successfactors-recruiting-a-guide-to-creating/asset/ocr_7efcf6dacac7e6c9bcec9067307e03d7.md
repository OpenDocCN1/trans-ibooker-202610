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

# Notes:

+   1. requestId is the SuccessFactors-generated primary key for the JobApplicationBackgroundCheckRequest entity and cannot be changed. There will be one or more requests per background request.
+   
+   2. vendorOrderId is the value set by the vendor as part of the synchronous response. You will use this vendorOrderId to send subsequent results.
+   
+   3. vendorCode is set during the outbound Integration Center set up in Manage Data. Please refer to step 8 of this chapter.