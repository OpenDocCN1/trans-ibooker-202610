![](bbox=[0.28, 0.08, 0.88, 0.73])

Figure 7.48: Step function workflow – Map function executing Athena queries in parallel

Enter the following parameters for the Athena StartQueryExecution API object:

API Parameters: Enter the following values. If you are using a different workgroup for specific features, update the input accordingly:

```
{ "QueryString.$": "$", "WorkGroup": "primary", "ResultConfiguration": {
```