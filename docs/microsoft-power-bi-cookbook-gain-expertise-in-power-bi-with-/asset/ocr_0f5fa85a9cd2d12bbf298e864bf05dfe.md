+   36. Repeat *step 32* for the **Product Tooltip** and **Customer Drill through** pages.
+   37. Use the **+** tab to add a page called **Information**.
+   38. On the **Information** page, use the **Insert** tab of the ribbon to create a **Text box** similar to the image in *Figure 4.55*.

![](bbox=[0.201, 0.174, 0.868, 0.335])

Figure 4.55: Information page of report with active mailto link

+   39. In this case, **Helpdesk** was selected and made into an active **mailto:** link.
+   40. From the **View** tab, choose **Lock objects** in the ribbon.
+   41. Select the **Europe** page and save your work.
+   42. From the **Home** tab, choose **Publish** in the ribbon to publish the report to the Power BI service.

# How it works

Once a report is ready to be published, it is a good idea to use the **Lock objects** setting to prevent unintended movement or resizing of visual elements. It is important to note, however, that **Lock objects** only affects the movement or resizing of visuals by mouse and keyboard. The **X Position**, **Y Position**, **Width**, and **Height** settings in the **General** section of the **Format** pane can still reposition and resize visuals.

Before publishing a report, you should hide or remove pages used during development, such as the **Scratch** page. Pages used for report page tooltips should also be hidden. Drillthrough pages may or may not be hidden, depending on their utility in the report. Hidden pages will not be shown to the report reader when viewing the report in the Power BI service.

It is often a good idea to include an **Information** page. Information pages can be used to describe the data sources used in the report, helpful tips for navigating the report, and even support information.

Positioning and alignment are key qualities in a professional report. As demonstrated in this recipe, Power BI provides many options for ensuring that visuals are aligned and sized in symmetrical ways. The automatic alignment guides are present whenever moving or resizing a visual element on a page.