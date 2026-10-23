![](bbox=[0.1, 0.1, 0.92, 0.23])

# Figure 7-11. Options for Calculating Field Values

The options for statements are shown as follows:

Field Value – Set the field value based on a specified field or fields as the operand(s) as well as an operation. Example:

+   - Set the “Days to Start Day” custom field using Start Date as the operand and Plus Age Calculation as the operator. This could then be used as a dashboard alert for applications of new hires who are nearing their start date.

If/Then – When the If conditions are true, then one or more statements are executed as a block. This may be as simple as setting a value. Example:

+   - If applications status is “Default” and “Previous Employee” is true, then set status to “Check R ehire E ligibility.” As this involves two conditions, the logic looks like this:
+     - If Application Status is equal to “Default,” then
+     - If Previous Employee is equal to true then
+     - Set application status to “Check Rehire Eligibility”

If/Then/Else – This is the same as If/Then statements, but a block of different statements can be executed if the condition is determined to be false.

Choose – This sets the field to be one of a series of values, depending on the value of the input field. For those from a programming background, it’s like a case statement. It avoids having a lot of nested if/then/else statements. Example:

+   - For applicants for a consulting job that have specified their willingness to travel on their candidate profile as either 10% or less, 10% to 25%, or 25% to 50%, put them in an “Unwilling to Travel” rejection status.