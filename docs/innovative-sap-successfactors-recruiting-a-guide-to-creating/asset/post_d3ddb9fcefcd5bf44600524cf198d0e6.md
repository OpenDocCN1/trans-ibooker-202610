
## CHAPTER 5 ENHANCING CANDIDATE ENGAGEMENT

Last, the candidate indicates they are finished chatting, which triggers the “log off” intent and requires no slots. We can see a visual representation of the intents and slots that define the scope of our chatbot as well as the subsequent scripting the chatbot executes as a result of each in Figure 5-3.

> **Note** *We have also included a `<locale>` slot to determine the language in which the chatbot and user are conducting intents for international implementations and because some objects within SAP SuccessFactors will require this information to query and create objects. Furthermore, `<country>` is required to determine which country’s consent statement will be shown to the candidate.*

![](img/879ce8537ecb7481ad77093e6fcbe574_119_0.png)

**Figure 5-3.** *Scope of Intents for Example Chatbot*

### Relevant OData API Objects and Functions

Now that we’ve identified what specific intents and slots will be in scope for our chatbot, we can identify the APIs and objects these intents and slots will call in SAP SuccessFactors. Let’s start with the job search intent. Similar to what we did in Chapter 2, we can use the Integration Center to explore the relevant OData objects and even start to build OData calls. Let’s get started!

### Job Search Intent

In order to gather the data needed to perform the job search intent and build the needed OData queries, follow the given steps:

1. Type and select “Integration Center” in the search bar.
2. The Integration Center main menu will appear. Click “My Integrations.”
3. Click the “Create” button.
4. In the drop-down that appears, select “Scheduled Simple File Output Integration.”

> **Note** You may wonder, why am I creating a file output if I want to make an OData call to feed information to my chatbot? The answer is that we will show you a trick to save you time building OData calls from scratch by using this type of integration in the Integration Center. Keep reading to find out!

5. Since we are interested in searching through active postings to accomplish our chatbot intent, we will start with the JobRequisitionPosting object. In the “Search for Entities by Entity Name” field, enter “JobRequisition” and click the first result on the left. The screen should appear as shown in Figure 5-4.

![](img/879ce8537ecb7481ad77093e6fcbe574_121_0.png)

**Figure 5-4.** *JobRequisitionPosting Object Fields*

6. Our search will also need to filter for only those postings that are currently public and active on the external career site. Select the “Job Posting Status” and the “Job Board ID” fields and click the “Select” button.

7. The Options screen will show as seen in Figure 5-5. Choose “Simple Header” from the drop-down in the “Header Type” field. Click the “Next” button.

![](img/879ce8537ecb7481ad77093e6fcbe574_121_1.png)

**Figure 5-5.** *Options Screen*

8. The Configure Fields screen will show. You will see a preview of the fields. You may notice a lot of postings for the internal career site or agencies, etc., or with a status value of expired. We want to make sure our chatbot only tells users about postings on the external career site that are active. Therefore, we will set some filters. Click the “Next” button to progress to the “Filter and Sort” screen.

9. Our first filter set will simply indicate that we only want to see postings for the external career site. Select “Job Board ID” for the Filter Set 1 “Field” column. Set “Operation” to “is equal to” and set the “Value” to “_external.”

10. Click the “Add Filter Set” to add another set of filter parameters. This second filter will ensure only active postings are shown and not ones that are deleted or expired. Select “Job Posting Status” for the “Field” column and “is equal to” for the “Operation.” Enter “Success” in the “Value” column. Click the “+” sign at the end of the row to add another parameter to the same filter set. Click the “Or” toggle beneath “Filter Set 2.” Enter “Job Posting Status” for the “Field” column, “is equal to” for “Operation,” and enter “Updated” in the “Value” column. The result should look like Figure 5-6.

![](img/879ce8537ecb7481ad77093e6fcbe574_122_0.png)

**Figure 5-6.** *Result of Entering Filter Parameters*

11. Click the “Previous” button to return to the “Configure Fields” screen. You should now only see results where the “Job Board ID” is equal to “_external” and the “Job Posting Status” is either “Updated” or “Success.” An example is shown in Figure 5-7.

> **Note** This assumes you have active, posted requisitions on the external career site in your SAP SuccessFactors system.

![](img/879ce8537ecb7481ad77093e6fcbe574_123_0.png)

**Figure 5-7.** *Example Filtered Results of Job Postings Object Query*

12. Great! We now have the results we want. Click the “Save” button and then choose “Save” to save your integration.

> **Note** If you want to download the .CSV file to see what the data will look like, you can follow the instructions in Chapter 2 to specify an SFTP location and execute the integration.

13. Next we will show you a trick where you can see the OData query the integration uses behind the scenes to gather this data. This is extremely useful to use since chances are whatever cloud chatbot

![](img/879ce8537ecb7481ad77093e6fcbe574_126_0.png)

**Figure 5-9.** *JobRequisitionLocale Fields*

8. The Options screen will appear. Click the “Filter and Sort” icon toward the top of the screen. We will only want to look through the list of active jobs we received in the last query that are also in the user’s language. In our example, we will just filter one job requisition ID and use en_US (the locale for United States English). Under Advanced Filters, select “Locale” under the “Field” column, “is equal to” under the “Operation” column, and enter the locale of your posting under the “Value” column (in our example, we use “en_US”). Click the “+” icon to the right of the row. Select “Job Requisition Id-JobRequisition” under the “Field” column, “is equal to” under the “Operation” column, and enter one of the requisition numbers from the previous query we built in the “Value” column (in our example, we use “2700”). Repeat as needed for all the requisitions returned in the last query. An example is shown in Figure 5-10.

**Figure 5-10.** *Filter Example for JobRequisitionLocale*

![](img/879ce8537ecb7481ad77093e6fcbe574_127_0.png)

9. Click the “Configure Fields” icon from the top to navigate back to the preview of fields. You should see your results. An example is shown in Figure 5-11.

![](img/879ce8537ecb7481ad77093e6fcbe574_127_1.png)

**Figure 5-11.** *Example Results of JobRequisitionLocale Filters*

10. Click the “Save” button in the bottom left-hand corner, and in the menu that appears, click “Save.” In the pop-up that appears, click “Save.”

11. Once again, we will want to grab the OData URL call in the integration specification file. To download the integration specification, click “Save” and then click “Export Integration Specification.”

12. Open the file with your text editor. You should see the generated OData call URL. An example is shown as follows:

```
/odata/v2/JobRequisitionLocale?$select=jobReqLocalId,
externalJobDescription,externalTitle,locale,
jobRequisition/jobReqId&$expand=jobRequisition&$filter=
(locale eq 'en_US' and jobRequisition/jobReqId eq 2700)
```

You now have the second piece of the puzzle that allows your chatbot access to the content of the job postings and relay it to the candidate on the career site! With this, we have all of the information from SAP SuccessFactors to complete our job search intent! In the next section, we will look at the create profile and job alert intent.

> **Note** You can sort through this content using your own search patterns for the search term (in our example, “Project Manager”), or you could use the OData $search parameter. For more information see, the API specifications from SAP here: https://api.sap.com/api/RCMJobRequisition/resource.

### Create Profile and Job Alert Intent

The second intent in scope for our chatbot that will require an OData interaction with SAP SuccessFactors is the create profile and job alert intent. In this intent, the chatbot gathers the user’s first name, last name, email address, mobile phone number, and their consent to create an ID and be contacted about future job opportunities relating to their search term. This involves making an OData call to create a candidate profile. Similar to what we did in Chapter 2, we will walk through the Integration Center to see what an example of candidate data looks like in our system and then create a call to create data in the proper format. Remember, it is best practice to know how the system will want
