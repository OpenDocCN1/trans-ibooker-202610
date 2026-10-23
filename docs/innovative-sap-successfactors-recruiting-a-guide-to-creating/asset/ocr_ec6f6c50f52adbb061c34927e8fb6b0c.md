Let’s assume that you’re a partner implementing a tax screening assessment as part of WOTC (Work Opportunity Tax Credit). By following the steps described in this chapter, you will be able to set the customer’s instance to accept WOTC qualification. You will be performing several steps, some of which are done in Provisioning and some in Admin Center. We have called out where each step will be set up.

Navigate to Provisioning ► Company Settings. Under Recruiting settings, ensure that Assessment Integration is enabled as shown in Figure 8-1. This step is probably not required, but you should check it, nevertheless.

![](bbox=[0.07, 0.305, 0.89, 0.47])

Figure 8-1. Check Provisioning Settings

# Step 2: Provisioning – Enable Assessment Integration Feature

You have to enable the feature permission for assessments on the job requisition template and for each applicable status. In the following example, assessment integration is enabled for the “Default” status. Ensure that you upload the modified job requisition template(s) in Provisioning.

```
<feature-permission type="assessmentIntegration">
    <description>![CDATA[The following roles can launch interview assessment during statuses with an Assessment category]]</description>
    <role-name>![CDATA[S]]</role-name>
    <role-name>![CDATA[O]]</role-name>
    <role-name>![CDATA[R]]</role-name>
    <role-name>![CDATA[G]]</role-name>
    <role-name>![CDATA[V]]</role-name>
```