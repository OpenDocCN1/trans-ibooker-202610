![](bbox=[0.08, 0.1, 0.88, 0.3])

# Figure 3-11. Requisition Question Output Example

Integration Center is designed for simple outputs, and we have demonstrated a limitation. We will use Postman to get a more complete picture of the JobRequisitionScreeningQuestion and JobReqScreeningQuestionChoice entities.

# Requesting Job Requisition Question Choices Using Postman

In Chapter 1, we used Postman to issue an ODATA call to SAP SuccessFactors to retrieve some job requisition information. In this example, we’re going to pull entities that are associated with that job requisition. We will request questions associated with a job requisition in addition to the question choices for multiple-choice questions.

Log into SAP SuccessFactors Recruiting test or demo environment, and find a job requisition that has more than question associated with it. There should be at least one question that is a multiple-choice question. If you don’t have the data, either create a new requisition with questions or add questions to an existing requisition. Take note of the requisition ID.

Open the Postman application and ensure that the environment that you created in Chapter 1 is selected as shown in Figure 3-12. A common mistake with multiple environments is to think that the data or the http request is wrong, when actually the wrong environment is being used.