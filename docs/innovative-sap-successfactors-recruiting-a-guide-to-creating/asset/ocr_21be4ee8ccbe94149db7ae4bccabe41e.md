# CHAPTER 1    INTRODUCTION

ODATA REST services are lightweight and used for direct interaction with database tables. They are stateless, meaning communication sessions with asynchronous back and forth are not supported. Every request results in a simple response.

# Data Model Navigation

ODATA entities can be thought of as database tables. For online stores, a Customer entity is tied to Orders, which in turn are tied to Products. In SAP SuccessFactors Recruiting, Job Requisitions are tied to Applications, which in turn are tied to Candidates.

Whereas SOAP web services are designed to be read by techie humans, using XML elements such as RequestInterviewResults that are recognized across the HR tech industry, ODATA directly represents the data model, using a generic way of representing entities.

Using a web service RequestInterviewResults might pass a job requisition ID and return interview results for each candidate. The response might look something like this:

```
```xml
<InterviewResults>
    <InterviewAssessment>
        <Candidate>
            <CandidateID>1234</CandidateID>
            <Name>Charlie Brown</Name>
        </Candidate>
        <InterviewDate>12/21/214</InterviewDate>
        <InterviewScore>A-</InterviewScore>
    </InterviewAssessment>
    <InterviewAssessment>
        <Candidate>
            <CandidateID>4321</CandidateID>
            <CandidateName>Lucy</CandidateName>
        </Candidate>
        ....... Etc.
    </InterviewAssessment>
</InterviewAssessments>
```
```