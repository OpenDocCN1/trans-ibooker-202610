With ODATA, we navigate entities using our request parameters rather than it being handled by predefined logic that drives the web service. Using the same example earlier, we could have the data model navigation as shown in Figure 1-1.

![](bbox=[0.14, 0.19, 0.86, 0.32])

Figure 1-1. Data Model Navigation from Job Requisition to Interview Assessment

CRUD (Create, Read, Update, Delete) operations are performed on ODATA entities. There are hundreds of entities across SAP SuccessFactors. This can be both powerful and confusing. Fear not, documentation and this book are your friends.

In the SAP Help Portal, the following useful guides give further information:

+   - The SAP SuccessFactors HXM Suite OData API: Developer Guide (Dev Guide)

+   - The SAP SuccessFactors HXM Suite OData API: Reference Guide (Reference Guide)

# ODATA Data Model Example

Now that we’ve described how ODATA works, let’s look at the ODATA model by diving into the SAP SuccessFactors system. An interactive view of the data model can be seen within the ODATA Data Model. In the search bar at the top right of the screen, type “ODATA API Data Dictionary.” Options will appear as soon as you start typing.

When the Data Dictionary screen appears, click Entity and then select Tag Recruiting (RCM) and Name JobRequisition. This will show the screen in Figure 1-2.