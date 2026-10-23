# Use Case: Candidate Sourcing

Let’s assume that you’ve been hired by a customer to act as a candidate sourcing platform. With the information you’ve read in this chapter, you should be in a position to build a working integration to supply candidates to the customer’s recruiting system. Let’s call this candidate sourcing platform MYTHICAL.

Let’s begin with a couple of assumptions before embarking on such an integration solution:

+   - The customer wants you to bring in external candidates from the MYTHICAL platform. In addition, the customer wants you to provide updates to existing candidates.
+   - You are expected to use OAuth2.0 to comply with the security requirements of the customer and SAP SuccessFactors.
+   - The job requisitions should be posted externally first before you can source candidates.
+   - Leveraging ODATA APIs, you will create candidate records in SuccessFactors in addition to any supporting attachments such as resumes and cover letters.
+   - The candidates should be aware of any new job requisitions posted by the customer in near real time.
+   - You will design the integration in such a manner as to make it as standard as possible so that you can apply the same principles to multiple customers with minimal customization.
+   - You will be given permissions to the various recruiting entities by the customer. The sample permissions are listed in the beginning of this chapter.
+   - At a bare minimum, the MYTHICAL platform has the ability to receive SOAP messages from SuccessFactors. A nice feature to have will be the ability of the platform to receive JSON messages from SuccessFactors (in case you are going to combine Intelligent Services with Integration Center).