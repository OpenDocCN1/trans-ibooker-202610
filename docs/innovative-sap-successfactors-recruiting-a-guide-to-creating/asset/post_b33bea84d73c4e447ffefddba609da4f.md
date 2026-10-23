
## CHAPTER 1 INTRODUCTION

![](img/879ce8537ecb7481ad77093e6fcbe574_18_0.png)

Figure 1-2. ODATA Data Dictionary List of Entities Related to JobRequisition

Select JobRequisition and the entity will be shown in detail, with the definitions of each field as shown in Figure 1-3.

![](img/879ce8537ecb7481ad77093e6fcbe574_19_0.png)

Figure 1-3. The Fields of the JobRequisition Entity in the ODATA Data Dictionary

In addition to showing the fields within the JobRequisition entity, the supported operations are shown. These are Query, Insert, Merge, Replace, Upsert, and Delete. Not all operations are supported for all entities. The possible operations for an entity can be found in the ODATA Data Dictionary as well as the Reference Guide.

The second way that we will be navigating the data model is through Integration Center. This will be described in Chapter 2.

### How to Execute Simple ODATA Calls

### Prerequisites

We can quickly get a list of all entities while confirming that we can access the ODATA API using a username and password shared with us by a system administrator. You will also need the SAP SuccessFactors Company ID.

The endpoint URL to access the ODATA APIs depends on the data center housing your SAP SuccessFactors instance. We will be using a demo system for our examples. The endpoint is https://apisalesdemo8.successfactors.com/.

The data center URLs to access your data center can be found in knowledge base article (KBA) here <https://launchpad.support.sap.com/#/notes/2605498>. You will need an SAP user, such as an S-user to access this site.

To query a list of entities, the URL is <Endpoint>/odata/v2. Our URL is <https://apisalesdemo8.successfactors.com/odata/v2>.

You will be prompted for a username and a password. The username is <user@companyid>. The user should be an integration user with limited access to the system. If you are responsible for creating roles and users, read the section “Setting Up Required Users and Permissions.”

### Setting Up Required Users and Permissions

Integration users should have just enough permissions to make ODATA requests and nothing more. They should not have access to any front-end functions that a human user would perform. They should also have a password that never expires as an expiring password would cause a failure of any API calls that SuccessFactors receives.

A separate role-based permission (RBP) role and group should be created for the integration user. It’s assumed that if you’re reading this book, you know the basics of role-based permissions. Figure 1-4 shows the permissions recommended for a recruiting integration user. Not all ODATA permissions may be needed. Foundation or MDF object permissions such as Location may also need read access.

| Role Permission Section | Permission | Comments |
| :------------------------- | :------------------------- | :------------------------- |
| General User Permissions | User Login | |
| | Login Method (Password) | If SSO is enabled |
| | Permission to Create Forms (check any requisition templates) | If requisitions are to be created using integration |
| Recruiting Permissions | Check off all ODATA permissions | |

![](img/879ce8537ecb7481ad77093e6fcbe574_20_0.png)

If you are performing API testing from your computer and are the only person using that integration user, then an expiring password is not such a big deal. In a production environment, it is disastrous for integrations. An exception needs to be made to the password expiration policy.

To set a password expiration exception, type “Password” in the action search in the top right of the SuccessFactors screen. “Password & Login Policy Exceptions” will be one of the options. The search tool within Admin Center can also be used as shown in Figure 1-5.

Admin Center

![](img/879ce8537ecb7481ad77093e6fcbe574_21_0.png)

Figure 1-5. The Password & Login Policy Settings Screen

Click “Set API login exceptions.” You can now add an exception by clicking the “Add” button. Enter the integration username, and set the maximum password age to “-1” for no expiration or another period depending on the policy of your organization. An IP address restriction is required. This limits the IP addresses from which the user can make calls to your SuccessFactors instance. An example would be the IP address of an assessment solution calling SuccessFactors.

### Calling SuccessFactors from a Browser

Open your favorite browser and type the ODATA endpoint for your SuccessFactors instance. For our demo instance, it’s https://apisalesdemo8.successfactors.com/odata/v2. You will be prompted for a username and password. The username format is username@companyid. After clicking Sign In, you will see a list of all ODATA entities like the one in Figure 1-6. Congratulations! You have executed your first ODATA API request.

This XML file does not appear to have any style information associated with it. The document tree is shown below:

```xml
<?xml version="1.0" encoding="utf-8"?>
<workspaces>
  <collection title="Default" />
  <collection title="GoalComment_1" />
  <collection href="BenefitProgramEnrollmentDetail" title="BenefitProgramEnrollmentDetail" />
  <collection href="JobRequisitionPostingFieldControls" title="JobRequisitionPostingFieldControls" />
  <collection href="GoalPermission_101" title="GoalPermission_101" />
  <collection href="InnerMessage" title="InnerMessage" />
  <collection href="JobApplicationStatusLabel" title="JobApplicationStatusLabel" />
  <collection href="BenefitInsuranceDependentDetail" title="BenefitInsuranceDependentDetail" />
  <collection href="ReportSmartMapping" title="ReportSmartMapping" />
  <collection href="SpotAwardProgramAdvancedSettings" title="SpotAwardProgramAdvancedSettings" />
  <collection href="TodoAction" title="TodoAction" />
  <collection href="UserResult" title="UserResult" />
  <collection href="InterviewOverallAssessment" title="InterviewOverallAssessment" />
</workspaces>
```

Figure 1-6. Results from a Simple ODATA Request

What did we really just do? We executed a *GET* request, meaning we asked for data without requesting any changes to the SuccessFactors database. A POST request would be used to make such changes. We didn’t specify a type of entity that we wanted to look at, such as requisition. Every ODATA request is done using an http(s) URI (Uniform Resource Identifier). We didn’t specify any parameters to say which data we wanted to return, so the full data model was returned. A browser can be used exclusively to perform requests. Once you start working with different environments, stringing requests along so that request parameters are based on the results of previous requests, and other needs such as automated testing, you need to invest in an API app, as we’ll see in Section 1.3. The small learning curve of becoming familiar with an app is far outweighed by the benefits.

### Example: Reading a Requisition Using Postman

Our goal is to retrieve all requisitions that have a status of “Approved.” We will read the *JobRequisition* entity, and filter on a field labeled “Internal Status.” The field name is *internalStatus*. The value of this field is changed when requisitions are created, approved, and closed.

### Getting Started with Postman

There are many desktop applications and browser-based tools for the testing and automation of interaction with HTTP APIs. We are going to use Postman as it has a free version and is one of the more commonly used ones. It’s also great for collaboration. You may be working with a colleague such as an integration consultant or back-end web developer. Being able to quickly build API calls and export them for use by your colleague can be a great productivity boost.

The Postman app can be found here www.postman.com/downloads/. It is available for Mac, Windows, and Linux. There is also a web version of Postman. We shall demonstrate using the desktop app. We will teach just enough Postman to perform the necessary API calls but recommend you check out the Postman Learning Center at https://learning.postman.com/docs/getting-started/introduction/. Figure 1-7 shows the initial view of Postman.

![](img/879ce8537ecb7481ad77093e6fcbe574_23_0.png)

Figure 1-7. Initial View of Postman or by Clicking New

We are going to start by creating a New Environment. This means we won’t have to remember the SuccessFactors API URI. In the future, it will also make it simple to switch between demo or customer environments.

Click “Environment” and then change the name from New Environment to something meaningful like “Mark’s Demo Environment.” Below that, add a variable name of Host and an initial value of the ODATA API URL for your datacenter, such as https://apisalesdemo8.successfactors.com/odata/v2. Add a variable for username, making it user@companyid.
