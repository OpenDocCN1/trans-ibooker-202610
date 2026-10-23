# Figure 9-16. Results of Requesting Attachment Using Postman

The file content is not shown in the figure. It is binary form and doesn’t make for interesting reading, unless you are an AI-based resume-to-requisition ranking system.

# 4) Update Application in SuccessFactors

The work of calculating a score from a job description and resume is then done by the matching system. This will take many steps, the first of which may be removing any resume information that could cause bias in the ranking engine, such as name, address, age, and photo (in some countries).

Once we have that magic matching score, we will ask SuccessFactors to update it, along with a hyperlink field which will take the recruiter to the details of that matching in the matching engine. We will illustrate the integration in Integration Center as if we were using a file-based import. After that, we will show how to do it in Postman.

```
<?xml version="1.0" encoding="utf-8"?>
<entry xmlns="http://www.w3.org/2005/Atom" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xml:base="https://apisalesdemo8.successfactors.com:443/odata/v2/">
  <id>https://apisalesdemo8.successfactors.com:443/odata/v2/Attachment(8811)</id>
  <title type="text"></title>
  <updated>2021-09-09T00:30:10Z</updated>
  <author>
    <name></name>
  </author>
  <link rel="edit" title="Attachment" href="Attachment(8811)"></link>
  <link rel="http://schemas.microsoft.com/ado/2007/08/dataservices/related/userNav" type="application/atom+xml;type=entry" title="userNav" href="Attachment(8811)/userNav"></link>
  <link rel="http://schemas.microsoft.com/ado/2007/08/dataservices/related/candidateNav" type="application/atom+xml;type=entry" title="candidateNav" href="Attachment(8811)/candidateNav"></link>
  <category term="SFODataservice.Attachment" scheme="http://schemas.microsoft.com/ado/2007/08/dataservices/schema"></category>
  <content type="application/xml">
    <m:properties>
      <d:attachmentId m:type="Edm.Int64">8811</d:attachmentId>
      <d:country m:null="true"></d:country>
      <d:fileName>Rick Resume (002).docx</d:fileName>
      <d:lastModifiedDateTime m:type="Edm.DateTimeOffset">2018-03-18T20:59:01Z</d:lastModifiedDateTime>
      <d:ownerIdType>CANDIDATE_ID</d:ownerIdType>
      <d:documentType m:null="true"></d:documentType>
      <d:deletable m:type="Edm.Boolean">false</d:deletable>
      <d:description m:null="true"></d:description>
      <d:language m:null="true"></d:language>
      <d:mimeType>application/vnd.openxmlformats-officedocument.wordprocessingml</d:mimeType>
      <d:moduleCategory>RESUME</d:moduleCategory>
      <d:donneeId></d:donneeId>
      <d:deprecable m:type="Edm.Boolean">false</d:deprecable>
    </m:properties>
  </content>
</entry>
```