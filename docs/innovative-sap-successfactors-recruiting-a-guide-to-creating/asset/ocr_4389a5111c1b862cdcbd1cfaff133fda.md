# CHAPTER 8  ASSESSMENT INTEGRATION FRAMEWORK

![](bbox=[0.074, 0.11, 0.89, 0.47])

**Figure 8-8.** *Setting Up an External Event*

# Notes:

**Event Type** – It should be shown as seen in Figure [8-8](#). The other values in this drop-down are for backward compatibility.

**Subscriber** – It is the same as the Subscriber Id in the previous screen.

**Protocol** – It is only SOAP_OVER_HTTP_HTTPS.

**Endpoint URL** – This is the endpoint of the partner’s system that will receive this assessment notification.

**Authentication** – There are three options, NONE, BASIC, or CLIENT_CREDENTIALS. Basic Authentication will be deprecated by the end of 2021 and not supported beyond 2022.

The following fields are required if your authentication is BASIC:

**User** – This is the userId required by the vendor’s software to enable integrations from external sources, for example, SAP SuccessFactors Recruiting.

**Password** – Self-explanatory.