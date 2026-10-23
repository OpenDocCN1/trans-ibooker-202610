# Step 3: Provisioning – Update the Job Requisition Template

Export the desired job requisition template, and ensure that the picklist field definitions are included in the job requisition template. Sample code is shown as follows:

```
<field-definition id="bgAccount" type="picklist" required="false" custom="true">
  <field-label><![CDATA[Background Check Account ID]]></field-label>
  <field-label lang="en_US"><![CDATA[Background Check Account ID]]></field-label>
  <field-description><![CDATA[Background Check Account ID]]></field-description>
  <field-description lang="en_US"><![CDATA[Background Check Account ID]]></field-description>
  <picklist-id>bgAccounts</picklist-id>
</field-definition>
<field-definition id="bgPackage" type="picklist" required="false" custom="true">
  <field-label><![CDATA[Background Check Package ID]]></field-label>
  <field-label lang="en_US"><![CDATA[Background Check Package ID]]></field-label>
  <field-description><![CDATA[Background Check Package ID]]></field-description>
  <field-description lang="en_US"><![CDATA[Background Check Package ID]]></field-description>
  <picklist-id>bgPackages</picklist-id>
</field-definition>
```

Note that the field-permissions section is omitted from the sample. The two fields must be added to the appropriate field-permission sections in the job requisition XML template.

Ensure that the feature permission for background check is also included.