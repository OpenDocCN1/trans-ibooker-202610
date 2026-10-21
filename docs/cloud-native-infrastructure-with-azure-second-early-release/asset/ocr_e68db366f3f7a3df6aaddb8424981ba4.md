# Azure Policy

While it is undoubtedly useful to actively run software that protects your cloud-based infrastructure, Azure Policy allows you to set a comprehensive policy against many aspects of your cloud infrastructure, particularly around resource security.

Azure Policy performs three key functions:

+   - Creates policies that prevent or enforce a certain configuration
+   - Allows for reporting of compliance against your own policy as well as industry benchmarks
+   - Brings your resources to compliance through bulk remediation for existing resources and automatic remediation for new resources

Potential use cases for Azure Policy include implementing governance for resource consistency, regulatory compliance, security, cost, and management.

Azure Policy definitions are defined in a JSON format and applied in a hierarchical manner; this means that one policy could be applied to one management group (and all the subscriptions/resources in it) and a more lenient policy could be set to a separate management group (and all the subscriptions/resources in it).

# Azure Policy Quickstart

Azure contains some built-in policies by default. You can view them by running this command in your terminal:

```
$ az policy definition list
```

Or you can view them in the Portal (Figure 8-9).