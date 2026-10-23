
## Overview of SuccessFactors to SuccessFactors Integrations

Integrations that use the SAP SuccessFactors solution as both the source type and destination type can be best illustrated in Figure 7-2. This is the flow shown on the Review and Run screen of your integration when selecting the SuccessFactors to SuccessFactors integration type.

![](img/879ce8537ecb7481ad77093e6fcbe574_167_0.png)

Figure 7-2. Integration Flow with SAP SuccessFactors Solution as Both Source and Destination Type

The integration flow performs the following:

- **Query** – Beginning with the starting entity (in our example, this will be JobApplication), include all of the fields that need to be read, either directly from the JobApplication entity or via associated entities such as JobApplication.
- **Filter** – Read just the desired data using filters, such as the status of the job application or the status set of the application (e.g., experienced hires status set vs. intern status set).
- **Map** – Map the source fields to the destination fields. The fields will often be the same, for example, JobApplication-Application ID to Application ID. Some destination fields may be calculated fields or fixed values.
- **Validate** – Before updates to the destination system are made, validation must occur. Examples include a check of whether picklist values are valid, mandatory fields are present, etc.

To create a SuccessFactors to SuccessFactors integration (meaning the same SuccessFactors instance), go to the Integration Center home page, select My Integrations. Then in the top right, click “Create” and then “More Integration Types” as in Figure 7-3.

![](img/879ce8537ecb7481ad77093e6fcbe574_168_0.png)

### Figure 7-3. Menu Path to Create More Integration Types

Select the Trigger Type of Scheduled since we want this to be an ongoing automation. Select SuccessFactors as Destination Type and Source Type. The correct options are shown in Figure 7-4.

![](img/879ce8537ecb7481ad77093e6fcbe574_168_1.png)

### Figure 7-4. Choose Integration Type

On the next screen, you select the starting ODATA entities, as we have done in previous chapters. You can search for entities by entity name or field name. When processing applications, JobApplication will likely be our starting entity. Select the entity after it shows in the search results on the left side. All fields and navigations are preselected and cannot be changed. Complete this step by clicking “Select” in the bottom right. The selection of the JobApplication entity is shown in Figure 7-5.

![](img/879ce8537ecb7481ad77093e6fcbe574_169_0.png)

**Figure 7-5.** *Selection of JobApplication Entity*

Once the starting entity has been chosen, you can see the configuration steps at the top of the screen, shown in Figure 7-6. You will notice that there are no destination settings. This is because you have selected SuccessFactors as both the Destination Type and Source Type.

![](img/879ce8537ecb7481ad77093e6fcbe574_169_1.png)

**Figure 7-6.** *Configuration Steps in Integration Center*

![](img/879ce8537ecb7481ad77093e6fcbe574_171_0.png)

### Figure 7-8. Configure Fields Page in Integration Center

Note that the destination fields are here shown on the left side.

The field options for populating fields are shown from left to right, as highlighted in Figure 7-9. They are

- Set as Fixed Value Field - Set a default value.
- Set as Associated Field - Map to another field. This can also be done from the Field Mapping View.
- Set as Calculated Field - Perform calculations, If/Then conditions, and more to populate the field.
- Associate Operand with a Field - This allows the editing of calculated field, associated fields for mapping, and more.

![](img/879ce8537ecb7481ad77093e6fcbe574_171_1.png)

### Figure 7-9. Icons for Populating Fields in Integration Center

We have already covered field mapping using Set as Associated Field. This is shown with the Field Mapping View, as shown in Figure 7-10. Note that the left side are source fields and the right side are destination fields. Source fields can be dragged from the left to right to assign them to the destination fields.

![](img/879ce8537ecb7481ad77093e6fcbe574_172_0.png)

Figure 7-10. Field Mapping View in Integration Center

Calculated fields and filters are very important for recruiting automation. Let’s take a look at the use of calculated fields. Figure 7-11 shows the different options for calculating field values. Note that there can be multiple rows of statements. Statements can be nested and reside on different levels.

![](img/879ce8537ecb7481ad77093e6fcbe574_173_0.png)

### Figure 7-11. Options for Calculating Field Values

The options for statements are shown as follows:

Field Value – Set the field value based on a specified field or fields as the operand(s) as well as an operation. Example:

- Set the “Days to Start Day” custom field using Start Date as the operand and Plus Age Calculation as the operator. This could then be used as a dashboard alert for applications of new hires who are nearing their start date.

If/Then – When the If conditions are true, then one or more statements are executed as a block. This may be as simple as setting a value. Example:

- If applications status is “Default” and “Previous Employee” is true, then set status to “Check Rehire Eligibility.” As this involves two conditions, the logic looks like this:
  - If Application Status is equal to “Default,” then
  - If Previous Employee is equal to true then
  - Set application status to “Check Rehire Eligibility”

If/Then/Else – This is the same as If/Then statements, but a block of different statements can be executed if the condition is determined to be false.

Choose – This sets the field to be one of a series of values, depending on the value of the input field. For those from a programming background, it’s like a case statement. It avoids having a lot of nested if/then/else statements. Example:

- For applicants for a consulting job that have specified their willingness to travel on their candidate profile as either 10% or less, 10% to 25%, or 25% to 50%, put them in an “Unwilling to Travel” rejection status.

**For Each Entity** – This loops through an associated entity such as Application ▶ Candidate ▶ Education and checks values. It is often used for Employee Central, looping through multiple pay components. A recruiting use case could be checking if an applicant has ever worked at a certain competitor and setting an application field based on that.

## Case Study: Advancing Candidates Based on Attributes

Our client has a lot of challenges in effectively sourcing certain groups of talent. One such group is highly qualified candidates with project management experience. One of the greatest sources of talent is the existing talent pool of registered candidates and employees within the SAP SuccessFactors Recruiting solution. There are often great runner-up applicants, sometimes referred to as silver medalists, when somebody is hired into a position from a slate of candidates. Recruiters would like visibility into whether an applicant is a silver medalist for another project manager role. They don’t want to dig for the information or navigate through several clicks, due to the sheer volume of applications.

## Technical Solution

Some options for identifying silver medalist candidates include looking at the Jobs Applied portlet on the application and/or by using a “Project Manager Silver Medalist” tag on the candidate profile. Another great way of grouping and marketing to these candidates is by putting them into a “Project Manager Silver Medalist” talent pool. This talent pool can then be used in targeted email campaigns.

It is not currently possible to filter by a candidates’ association to a talent pool using Integration Center. For this reason, we are going to use candidate profile tags to identify silver medalist project managers. If we want to also track such candidates in a list, we can create a talent pool with an overnight search that assigns candidates based on the silver medalist tag. We then have the best of both worlds.
