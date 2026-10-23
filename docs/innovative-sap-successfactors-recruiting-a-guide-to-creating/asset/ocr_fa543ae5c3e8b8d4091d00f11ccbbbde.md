```
{
    "description": null,
    "descriptionMessageKey": null,
    "hasValueAlways": false,
    "name": "isOnboardingInitiated",
    "type": "STRING"
}
]
},
    "publisher": "Recruiting",
    "topic": "com.successfactors.recruiting.JobRequisition.Updated",
    "type": "JobRequisition.Updated"
}
```

Once your subscription is configured and the connection to the external system is successful, the payload is delivered via a SOAP message shown as follows:

```
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<ns6:ExternalEvent xmlns:ns6="http://notification.event.successfactors.com" xmlns:ns5="http://schemas.xmlsoap.org/soap/envelope/" xmlns:ns7="com.successfactors.event.notification" xmlns:ns2="com.successfactors.alert" xmlns:ns4="http://alert.successfactors.com" xmlns:ns3="http://www.boomi.com/connector/wss">
    <ns6:externalEventMeta>
        <ns6:externalEventId>b772edb5-9f59-4b53-8595-6e45d08c0756</ns6:externalEventId>
        <ns6:type>com.successfactors.recruiting.JobRequisition.Updated</ns6:type>
        <ns6:publishedAt>1621793760938</ns6:publishedAt>
        <ns6:publishedBy>sfadmin</ns6:publishedBy>
        <ns6:repost>false</ns6:repost>
    </ns6:externalEventMeta>
    <ns6:events>
        <ns6:event>
            <ns6:eventId>fa477244-08c2-4293-aa65-2bad255b75d9</ns6:eventId>
            <ns6:entityType>JobRequisition</ns6:entityType>
```