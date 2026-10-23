![](bbox=[0.115, 0.107, 0.935, 0.503])

# Step 12: Candidate – View Email Notifications

If the assessment is configured for a status other than the “Brand New Application,” the candidate will receive an email described in step 8. The candidate will then click the APPLICATION_ASSESSMENT_PROVIDER_URL token to undergo the assessment and complete the assessment. The details of the assessment are described in the following.

# Step 13: Assessment Order – Request and Response

Upon successful creation of the Assessment Order, SAP SuccessFactors will create a SOAP Request and a synchronous SOAP Response is expected:

```
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<ns7:createRcmAssessmentRequest xmlns:ns6="http://schemas.xmlsoap.org/soap/envelope/" xmlns:ns5="com.successfactors.alert" xmlns:ns7="http://alert.successfactors.com" xmlns:ns2="http://www.boomi.com/connector/wss"
```