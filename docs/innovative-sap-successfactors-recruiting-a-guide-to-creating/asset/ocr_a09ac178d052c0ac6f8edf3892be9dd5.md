+   - The MYTHICAL platform also has the ability to invoke ODATA APIs against the customer’s Recruiting APIs.
+   - If for some reason, there’s a limitation on the MYTHICAL platform to consume events, you should make accommodations to address this limitation with the customer.
+   - For the purposes of this chapter, let’s assume that the candidate sourcing is limited to North America.

The steps involved in building a solution can follow the industry-accepted approaches to software development life cycle. Typically, the steps needed are Requirements Gathering and Analysis, Design, Implementation, Testing, Deployment, and Maintenance. A detailed treatment of the steps is outside the scope of this chapter. Given the assumptions and the customer’s requirements, you should be in a position to complete the first step, namely, Requirements Gathering and Analysis.

In order to complete the Requirements Gathering and Analysis, you may need to have several discussions with the prospective customer before you embark on the next step – Design. What objects/entities will you need access to? At a bare minimum, you need access to the following APIs, Candidate, JobRequisition, and JobApplication. Chapter 2 covered the basics of working with ODATA. Please refer to that chapter for more information.

Candidate Sourcing – Process Integration Flow:

+   1. Customer creates job requisitions, updates them, and posts them externally for sourcing.
+   
+   2. MYTHICAL platform will initially get all relevant job requisitions en masse. The amount of information about the job requisition will depend on the requirements of the MYTHICAL platform and typically a fraction of the information maintained with the SuccessFactors job requisition.
+   
+   3. Customers will periodically update the job requisitions.
+   
+   4. MYTHICAL platform will be notified of the updates to job requisitions by subscribing to the Update of Job Requisition Intelligent Services event and will subsequently update the job requisitions on the MYTHICAL platform.