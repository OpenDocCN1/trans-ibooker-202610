the value with whatever is in the file. We would need a middleware application to query the system for the current value, perform the calculation, and then update the field. You can learn more about middleware in the next chapter. If you have properly set up the configuration of your SAP SuccessFactors Recruiting system, the number of openings standard field count will be reduced by one automatically each time a candidate is put in the “Hired” status. Therefore, we will never need to send a negative/subtracting count from the payroll system on hire.

Now that we’ve prepared our file, we have established what the file will look like coming out of the payroll system. We can hand it off to the payroll system developers to use as a template for what to output. On our end, let’s use it to build our inbound interface into SAP SuccessFactors! Follow the steps to build the inbound interface in SAP SuccessFactors:

+   1. Type and select “Integration Center” in the search bar.
+   2. The Integration Center main menu will appear. Click “My Integrations.”
+   3. Click the “Create” button.
+   4. Choose “Scheduled CSV Input Integration” as shown in Figure 2-13.

![](bbox=[0.345, 0.515, 0.69, 0.655])

Figure 2-13. Create Scheduled CSV Input Integration

+   5. In the “Search for Entities by Entity Name” field, type “JobRequisition.”
+   6. Click the JobRequisition object that will appear as the first search result on the left as seen in Figure 2-14. Then click “Select.”