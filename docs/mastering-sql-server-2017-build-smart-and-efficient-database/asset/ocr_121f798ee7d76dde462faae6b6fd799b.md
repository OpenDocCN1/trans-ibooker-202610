The task base class provides two methods that you need to override, and in them provide your custom code to:

+   - Validate the configuration of the task. This method is called automatically by the SSIS control flow designer whenever the task settings are changed, and allows you to communicate with the SSIS package developers to help them understand the task’s configuration and help them configure it correctly before it can be used.
+   - You also need to provide the logic needed to execute the task. This method will be called at runtime, when the package execution reaches the task in the control flow sequence. This method also allows access to various SSIS package resources:
+     - The SSIS variables can be read or written
+     - The connections accessible to the package can be used to connect to various data sources
+     - The custom task may send log entries to the event log
+     - You are encouraged to implement events in your custom task, so that its performance can be captured by SSDT at design time, and by the SSIS server after the packages implementing the task have been deployed
+     - In the custom task, you can also detect whether the operation is participating in a transaction, and use this information when connecting to a data source

Both the preceding methods return a DTSExecResult value specifying the result of the validation, or the execution; this communicates the outcome of each method to the SSIS package developer, as well as the SSIS execution engine. The following values are supported by the DTSExecResult enumeration:

+   - Success is used to specify that the validation, or the execution, completed successfully.
+   - Failure specifies that the validation, or the execution, has failed. Generally, in case of failure, additional events should be returned from the task to provide more information about the failure to the SSIS package developer, or to the administrator of the deployment environment.
+   - Completion can be used for executions when either Success or Failure are not relevant results, or not specific enough. For instance, if a task completes its work successfully it returns Success, if it fails, it returns Failure, but if no work was performed, even though the configuration was in order, the task might return Completion to communicate a specific result that is neither a success nor failure.