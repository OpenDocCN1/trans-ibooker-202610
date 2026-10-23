
## Figure 13-4. Selecting “Application Status Id” for the Filter

+   8. Next, enter the status ID for “Ready to Hire” in the “Value” column so that we only see applications in this status. An example is shown in Figure 13-5.

## Figure 13-5. Setting the Filter So That only “Ready to Hire” Applicants Are Shown in the Output

> Note You will need to look up the status ID in the Applicant Status Set. To do this, make sure you save your interface and then type and select “Edit Applicant Status Configuration.” Click the status set associated with the requisition template for which you are exporting applicants. You will see the status IDs for each status within the status set on the left. An example is shown in Figure 13-6.

![](img/879ce8537ecb7481ad77093e6fcbe574_346_0.png)

![](img/879ce8537ecb7481ad77093e6fcbe574_346_1.png)

![](img/879ce8537ecb7481ad77093e6fcbe574_347_0.png)

## Figure 13-6. Applicant Status Set Configuration Showing Status ID

9. Next, we had one field on the jobApplication that is a picklist that could be translated to many languages. Since the payroll system file wants the spelled out name of the state in English, we will have to grab this translated label using a special sub-filer. Click “+ Add Field.” Under the Entity Tree View, click “state,” then picklistLabels, and then “label.” Beneath, click the FieldFilters. Click “picklistOption” and choose <first> (since there will only be one option ever chosen, it is safe to default to the first). Then click “PicklistLabel” and choose “locale” under the “Field” column and type “en_US” in the “Value” column. Click the “Add Association” button. An example is shown in Figure 13-7.

![](img/879ce8537ecb7481ad77093e6fcbe574_349_0.png)

**Figure 13-8. Example Adding CostCenter Code from jobRequisition**

13.  In an international implementation, you may also have your data stored in different locales on the requisition. To add the job title in the “en_US” (for English United States), click the “+ Add Field” button. In the pop-up, under “Entity Tree View,” choose “jobRequisition,” then “JobReqLocale,” and the “Job Title.” Click “Field Filters” and then click “JobRequisitionLocale.” Choose “Locale” under the “Field” column and then type “en_US” under the “Value” column. Click the “Add Association” button. An example is shown in **Figure 13-9**.

## Figure 13-9. Adding the Translated “Job Title” Field from the “jobRequisition” Object

14. Next, we will add the salary field. Since we can have more than one jobOffer associated with a single jobApplication, we will need to also specify which jobOffer object to choose. Click “+ Add Field” again and then choose “jobOffer” on the left. Then choose “Salary Base” on the right. Click the “Field Filters” section on the bottom and then choose “<Last>” in the “Field” column. This will choose the last available JobOffer object created. Click “Add Association.” An example is shown in Figure 13-10.

![](img/879ce8537ecb7481ad77093e6fcbe574_350_0.png)

![](img/879ce8537ecb7481ad77093e6fcbe574_350_1.png)

## Figure 13-10. Adding Fields from Job Offer While Applying a Filter

15. Now that we have all of the fields we need, we can start reformatting the file to fit the specified format. First, let’s start by eliminating the fields we do not need (there are ID fields that have been automatically added as part of the navigations). Click the “Configure Fields” icon at the top.

16. Click the “Application Id” column at the top header and choose “Remove Field.” An example is shown in Figure 13-11. Repeat this step for the “Offer Approval Id-Job Offer,” “Job Requisition Id-JobRequisition,” and “id-PicklistOption” fields.

![](img/879ce8537ecb7481ad77093e6fcbe574_351_0.png)

Figure 13-11. Removing a Column

17. Next, drag and drop each column header so that they are in the desired order of the sample file. An example is shown in Figure 13-12.

![](img/879ce8537ecb7481ad77093e6fcbe574_352_0.png)

Figure 13-12. Final Column Order Example

18. Now let’s change the headers to match what is in the sample file. Click each column, and in the menu, set the “Label” field to the corresponding label from the sample file and hit enter. An example is shown in Figure 13-13.

![](img/879ce8537ecb7481ad77093e6fcbe574_352_1.png)

Figure 13-13. Relabeling Column Headers Example

19. We are almost there! Now let’s look at the data formatting. The first observation when we look at this preview screen with the sample data in our particular example system is that the RateOfPay column does not have values for every entry (this will vary in your system depending on the data in the system). This is a common issue as recruiters may have bypassed the offer approval step. One way to fix this and avoid a throwback from the payroll system is to make this a required field on the interface. Click the column header and choose “More Field Options.” In the pop-up that appears, check “Value Is Mandatory.” An example is shown in Figure 13-14.

![](img/879ce8537ecb7481ad77093e6fcbe574_353_0.png)

## Figure 13-14. Setting Column to Be Mandatory

> Note This will throw an error when the interface is run to help alert the system administrator that the field is missing. This is preferred so that a manual step can be taken to correct the issue – if we had simply filtered out the blank values, no one would know there was an issue and wonder why the candidates were never transferred to payroll!

20. Continuing to examine the example data on screen, the only item left is the cost center field and gender (you will notice TaxID is missing as well in Figure 13-13, but this is hidden in our screenshot for security purposes – we will assume the data is present). Looking at the sample data shown in the Integration Center screenshot in Figure 13-13, it seems perhaps the cost center codes in SuccessFactors have both a label and a code, whereas there is a different code in the payroll system. Similarly, “Gender” shows up as “Male,” “Female,” or “No Selection” instead of “M,” “F,” or blank. One way to remedy this is to create a lookup table. Click the name of the column on top and then click “Value Lookup Table.” In the pop-up, click “+ New.” Give the table a name, and then for each value you want to convert, fill in the original value in the “Input Code” column and the converted value in the “Output Code” column. An example is shown in Figure 13-15. Click “+” to add more rows to the table and fill in the columns for all values you want to convert. Click “Save” when you are done building your table. Then click “OK.” Repeat for the second column so that both “CostCenter” and “Gender” fields are remedied.

![](img/879ce8537ecb7481ad77093e6fcbe574_354_0.png)

Figure 13-15. Creating a Value Lookup Table

![](img/879ce8537ecb7481ad77093e6fcbe574_356_0.png)

## **Figure 13-17. _Example Run of Interface with Warnings_**

+   23. Click the timestamp of the last run to see the details of the run.  
+   In Figure 13-18, you can see the missing required values flagged a warning. These rows were excluded from the output (which is a good thing!), and the application numbers are provided so the system admin can contact the recruiters and get the applications corrected.

![](img/879ce8537ecb7481ad77093e6fcbe574_356_1.png)

## **Figure 13-18. _Example Warnings Thrown for Missing Fields_**

24. You can now download the final output file from the SFTP server and work with the payroll provider to test it out. An example is shown in Figure 13-19.

![](img/879ce8537ecb7481ad77093e6fcbe574_357_0.png)

Figure 13-19. Example Payroll Input File Built from Integration Center

Congratulations! You have now created a payroll input file in the required format! You should now be able to create a similar file based on whatever requirements your specific payroll system needs.

## Conclusion

In this chapter, we have focused on the final step in recruiting a candidate: hiring. We started by discussing some practical scenarios where recruiting data can be shared with various systems to help ease the hiring process. We then jumped into a review of the relevant data that is typically passed to other systems during the onboarding and hiring process. We wrapped up with a specific case study that walked through the specific steps we can take to set up an employee export in a specific format to be input into a payroll system. It is important to note that while this is probably one of the most practical and common scenarios, many others exist as discussed in the beginning of the chapter and can be implemented using a variety of methods covered throughout the book such as ODATA.
