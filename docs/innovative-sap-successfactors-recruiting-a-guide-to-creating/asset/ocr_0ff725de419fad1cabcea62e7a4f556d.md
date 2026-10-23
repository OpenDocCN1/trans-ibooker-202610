# CHAPTER 9\nADDITIONAL CUSTOM ODATA INTEGRATIONS

As can be seen in Figure 9-6, the steps to be performed are

+   1. Read open job requisitions that have a relevant job role, including job profile with description, competencies, etc.\n\n2. Read relevant applications for this job requisition and their resume, background information, etc. They may be filtered by their status (e.g., only new applications) and whether the third-party matching engine has already provided a matching score.\n\n3. Generate a matching score for each application in the third-party matching engine.\n\n4. Update the SAP SuccessFactors application with the matching score.\n\n5. If applicable, the recruiter gives feedback by clicking a link to the matching solution.

![](bbox=[0.084, 0.462, 0.886, 0.706])

**Figure 9-6.** *Steps in Our Automatic Matching Example*