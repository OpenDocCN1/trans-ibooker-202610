```
"type": "SFODa ta.JobRequisition",
"uri": "JobRequisition"
}
}
]
}
```

Now that we have created the requisition, our remaining task is to make a subsequent ODATA call back to the HRIS system (or middleware) to indicate the requisition creation was successful and provide the requisition ID. To do this, we will simply create another ODATA query as we have done twice now. Our query will only need to include the HRIS system unique identifier of the position (such as position ID) and grab the unique ID of the requisition.

+   1. Type and select "Integration Center" in the search bar.
+   2. Click "My Integrations."
+   3. Click the pencil icon next to the last export integration you created to edit it.
+   4. Click "Configure Fields" at the top and then click the bulleted list icon in the upper-right-hand corner.
+   5. Remove all fields except the requisition ID and position ID.
+   6. Add the fields "jobReqId" and "positionNumber" (or whatever unique ID is used to store the HRIS system position unique ID) by clicking the "+ Add" button and searching for the fields.
+   7. Click "Filter and Sort" at the top and then click "Advanced Filters." Enter "Position Number" in the "Field" column and the number of the position in the "Value" column. An example is shown in Figure 2-29.

> **Note**  This assumes a 1:1 relationship between position and requisition. Also you may wish to use a custom field to store a third-party HRIS position number rather than the standard position field since this standard field is used by Employee Central.