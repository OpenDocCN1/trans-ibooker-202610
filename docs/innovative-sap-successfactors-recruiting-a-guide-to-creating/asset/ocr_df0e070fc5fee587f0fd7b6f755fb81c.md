![](bbox=[0.26, 0.1, 0.71, 0.28])

# Figure 3-16. Custom MDF Object to Default Requisition Questions

In the preceding example, we’ve used the externalCode of “Global_18Years?” to make it easier to identify this question and that it’s valid for all requisition templates. Note that we’ve left Requisition Template ID blank to indicate that this question is valid for all requisition templates. We’re not specifying the question type or possible answers here. Instead, we are referencing the Question ID, which is the GUID for the question, which can be found by exporting the question library.

Maintaining the previous configuration will be an ongoing process performed by recruiting administrators. For this reason, it has to be secured using role-based permissions (RBP) per Figure 3-17.

![](bbox=[0.28, 0.53, 0.68, 0.72])

# Figure 3-17. Example Role-Based Permission for Question Defaulting Configuration