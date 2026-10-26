
7. Use the drop-down lists in the Source Column and Domain cells to map the following columns and domains:

| Column | Domain |
|---|---|
| BirthDate | BirthDate |
| StreetAddress | StreetAddress |
| City | City |
| StateProvince | State |
| CountryRegion | Country |
| EmailAddress | EmailAddress |
| Occupation | Occupation |

8. The following screenshot shows the correct mappings of columns to domains. When your mappings are correct, click Next:

![](img/07d1225e179d5df9f2b2d26b026386c6_473_0.png)

- 9. On the **Cleanse** tab, click **Start**. Wait until the computer-assisted cleansing is finished, then review the results of the profiling. Click **Next**.
- 10. On the **Manage and View results** tab, check the results, one domain at a time. Start with the `BirthDate` domain. There should be one invalid value. Make sure that the `BirthDate` domain is selected in the left-hand pane, and click the **Invalid** tab in the right-hand pane. Note the invalid value that was detected. You could write a correct value now in the **Correct to** cell of the grid with invalid values, but this action is not needed for this exercise. Note that all correct values were suggested as new.
- 11. Select the `StreetAddress` domain in the left-hand pane. One value should be corrected. However, because only the term-based relation (and not the whole value) was corrected, it does not appear among the corrected values. It should appear among the new values. Click the **New** tab in the right-hand pane. Search for the value `7800 Corrine Ct.` and note that it was corrected with 100 percent confidence to `7800 Corrinne Court`.
- 12. Clear the **Search Value** textbox. Select the `State` domain in the left-hand pane. Click the **New** tab in the right-hand pane. Note that one value (`Kingsland`) was found as new. The similarity threshold to the original value (`Queensland`) was too low for DQS to automatically correct or even suggest the value. You could correct this value manually, but this is not needed in this exercise.
- 13. Select the `City` domain in the left-hand pane. Two values should be corrected. Click the **Corrected** tab in the right-hand pane. Note the corrections of the synonyms for `München` (`Munich` and `Muenchen`) to the leading value (`München`). Note also that the confidence for these two corrections is 100 percent. All other values already existed in the KB, and therefore DQS marked them as correct.
- 14. Select the `Country` domain in the left-hand pane. One value should be suggested. Click the **Suggested** tab in the right-hand pane. Note that DQS suggests replacing `Austria` with `Australia` with 70 percent confidence. You can approve a single value by checking the **Approve** option in the grid. However, don’t approve it, because, of course, this is a wrong suggestion. Note that DQS identified all other countries as correct.
- 15. Select the `EmailAddress` domain in the left-hand pane. One value should be invalid. Click the **Invalid** tab in the right-hand pane. DQS tells you that the `jon24#adventure-works.com` email address does not comply with the `EmailRegEx` rule. Note that all other values are marked as new.

- 16. Select the **Occupation** domain in the left-hand pane. Note that all values are new. Click the **New** tab in the right-hand pane. Note that the value `Professional` is underlined with a red squiggly line. This is because you enabled the spelling checker for the Occupation domain. Enter `Professional` in the **Correct to** field for the incorrect row. Note that, because you corrected the value manually, the confidence is set to 100 percent. Select the **Approve** checkbox for this row. The row should disappear and appear among the corrected values. Click the **Corrected** tab. Observe the corrected value along with the reason. Click **Next**.
- 17. On the **Export** tab, look at the output data preview on the left-hand side of the window. You could export the results to a SQL Server table and then correct the original data. However, you don't need to export the results in this lab. Just click **Finish**.
- 18. Close SSMS and the Data Quality Client application.

## Creating a MDS model

In this recipe, you are going to create an MDS model with the entities, attributes, and hierarchies needed for a customer's entity set. Then you will populate the entities and check your business rules.

## Getting ready

In order to test this recipe, you need to have MDS installed. Please refer to the following link to learn how to install the MDS components:

`https://docs.microsoft.com/en-us/sql/master-data-services/install-windows/install-master-data-services`

## How to do it...

- 1. You need to open the Master Data Manager application. Open your web browser, navigate to your Master Data Manager site, and log in. Navigate to the home page.
- 2. Click the **System Administration** link.
- 3. In the **Manage Models** page, click **Add**.

4. Name the model AWCustomer. Make sure that the option Create entity with the same name as model is checked. Leave the Description textbox empty, and Log Retention drop-down list to the default, System Setting, as the shown in the following screenshot. Click the Save button:

![](img/07d1225e179d5df9f2b2d26b026386c6_476_0.png)

- 5. On the **Manage Models** page, click the **Entities** link in the **Go to** section.
- 6. On the **Manage Entities** page, click the **Add** button. Name the entity `StateProvince`. Check the **Create code values automatically** option. Write 1 in the **Start with** text box. Make sure that the **Enable data compression** checkbox is checked and the **Approval Required** is not checked. Click the **Save** button.
- 7. Click the **Manage Entities** link at the top left of the page. Create another entity, `CountryRegion`, with the same settings as the `StateProvince` created in the previous step. Save it.
- 8. Click the **Manage Entities** link at the top left of the page. Click the `StateProvince` entity in the list of entities to highlight it. Click the **Attributes** link in the **Go to** section.
- 9. Note that there are already two attributes created, the `Name` and the `Code` attributes.
- 10. Click the **Add** button. Name the new attribute `CountryRegion`. Make the new attribute domain-based and use the `CountryRegion` entity for the domain values. Do not change the display pixel width and do not enable change tracking. Save the attribute.
- 11. Start editing the `AWCustomer` entity. Select it in the **Entity** drop-down list. Add a domain-based leaf member attribute, `StateProvince`. Use the `StateProvince` entity for the domain of possible values. Do not enable change tracking. Save the attribute.
- 12. Add a free-form leaf member attribute, `StreetAddress`. Use the **Text** data type. Change the `Display Width (Pixel)` to 200. Do not enable change tracking. Save the attribute.
- 13. Add a free-form leaf member attribute, `City`. Use the **Text** data type. Leave the default length of 100 and the default pixel width of 100. Do not enable change tracking. Save the attribute.
- 14. Add a free-form leaf member attribute, `EmailAddress`. Use the **Text** data type. Leave the default length of 100 and the default pixel width of 100. Do not enable change tracking. Save the attribute.
- 15. Add a free-form leaf member attribute, `MaritalStatus`. Use the **Text** data type. Change the length to 1. Change the display pixel width to 20. Do not enable change tracking. Save the attribute.
- 16. Add a free-form leaf member attribute, `BirthDate`. Use the **DateTime** data type. Use the default pixel width of 100. Use the `yyyy/MM/dd` input mask. Do not enable change tracking. Save the attribute.

- 17. Add a free-form leaf member attribute, YearlyIncome. Use the **Number** data type with two decimals. Use the default pixel width of 100. Use the -#### input mask. Do not enable change tracking. Save the attribute.
- 18. Navigate to the **Manage Entities** page (**Home | AWCustomer Model | Manage Entities**). Select the `AWCustomer` entity.
- 19. Click the **Attribute Groups** link. Click **Add**. Name the group `Demography`.
- 20. In the **Attributes** section, add the `MaritalStatus`, `BirthDate`, and `YearlyIncome` attributes to this attribute group, as shown in the following screenshot, and then save the attribute group:

![](img/07d1225e179d5df9f2b2d26b026386c6_478_0.png)
