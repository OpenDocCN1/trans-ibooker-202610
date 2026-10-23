# Step 13

If the status ID isn’t recognized in our Choose statement, then it remains zero. After we have exited the For Each Entity loop, we will check if the status ID is still zero. If it is, we will set the new status to be the same as the old one. This means that the application will remain in the Default status. This is shown in Figure 7-22.

![](bbox=[0.107, 0.236, 0.929, 0.469])

# Figure 7-22. *If/Then Logic*

Those are our steps for setting up the calculated field, other than testing it. The logic of the calculated field can be checked by using the preview records (in this case Applications) and checking how each one is processed. This is done using the Calculation Trace.

# Use of the Calculation Trace

The Calculation Trace is very handy for troubleshooting. We will use it to verify that an application that we know should move to “Short List” has generated the correct status ID. For each entity, the logic is shown in the following, along with the evaluation of any conditions as being true or false. In Figure 7-23, we can see that there is a relevant silver medalist tag and that because the status ID for Default is 63, we then set the new status ID to be 68 (Short List).