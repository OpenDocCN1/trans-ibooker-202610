# CHAPTER 7　AUTOMATION USING INTEGRATION CENTER

![](bbox=[0.072, 0.108, 0.893, 0.239])

*Figure 7-20.  Defining the If/Then Statement*

# Step 12

We want to ensure that applications tagged as silver medalists jump the line and are moved directly to the Short List status. To do that, we update the application status using the application status ID. Application statuses belong in a Status Set, which represent a unique recruiting process. The status IDs for both Default and Short List status will be different for each Application Status Set. A client may have several different status sets to support different processes.

We are going to check the current application status ID. The number will let us know which status set it resides in, as they are unique. That will then tell us which “Short List” status ID to use.

For example, a Default status ID of 63 tells us that the status set is “Simple Status Set,” which has the code of 21. The “Short List” status in that status set is 68. In this example, we need to set the new status to 68 to move the candidate to the Short List status. If we try to set the status ID to something that doesn’t belong in this status set, the integration job will error out.

We will use the Choose statement to map the Default status IDs to the correct Short List status IDs. This can be seen in Figure 7-21.

![](bbox=[0.072, 0.689, 0.894, 0.837])

*Figure 7-21.  Mapping Status IDs to Status Set*