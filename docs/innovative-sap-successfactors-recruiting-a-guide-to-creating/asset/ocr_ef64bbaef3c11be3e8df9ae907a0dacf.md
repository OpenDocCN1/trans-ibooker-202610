# CHAPTER 12    INTELLIGENT SERVICES

Earlier in this chapter, we described how to get all the events programmatically by executing GET <<endpoint>>odata/v2/getExtEventMetaDataDefinition either in Postman or on the browser. Shown in the following is the response payload for the “Update of Job Requisition” event:

```
```json
{
  "description": "This event is raised when an approved or closed requisition is updated.",
  "effectiveDated": false,
  "entity": "JobRequisition",
  "entityKeys": {
    "results": [
      {
        "description": null,
        "descriptionMessageKey": null,
        "name": "jobReqId",
        "type": "STRING"
      },
      {
        "description": null,
        "descriptionMessageKey": null,
        "name": "startDate",
        "type": "DATE"
      }
    ]
  },
  "name": "Update of Job Requisition",
  "params": {
    "results": [
      {
        "description": null,
        "descriptionMessageKey": null,
        "hasValueAlways": false,
        "name": "updatedFieldsList",
        "type": "STRING"
      },
```
```