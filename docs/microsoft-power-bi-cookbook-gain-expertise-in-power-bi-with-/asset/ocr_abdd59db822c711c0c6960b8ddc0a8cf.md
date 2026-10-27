In the ribbon of the **View** tab, click the drop-down arrow in the **Themes** area and choose **Browse for themes**:

![](bbox=[0.133, 0.125, 0.866, 0.247])

Browse to the downloaded JSON file, select it, and click **Open**. A message will appear, stating that the theme was imported successfully. Theme settings will be applied to the report. The colors available for formatting visuals now reflect the theme's colors. Save CH4_R1.pbix.

![](bbox=[0.133, 0.349, 0.866, 0.557])

Themes cannot be applied in the Power BI service, and some custom visuals do not apply report themes. Additionally, report themes do not impact the colors of images and do not override any custom color applied to a specific data point in a visual. It is important to apply themes early when developing reports to avoid situations where hours of formatting are suddenly undone after applying a theme!

Each report theme JSON file has a structure of elements of color code mapping consistent with that shown in *Figure 4.6*.

```
```json
{
  "name": "ColorBlindSafe-Longer",
  "dataColors": ["#074650", "#009292", "#edc948", "#eb6ba4", "#490091", "#b5d6ff", "#b3dafe", "#6db6ff", "#914800", "#23cfd3"],
  "background": "#FFFFFF",
  "foreground": "#074650",
  "tableAccent": "#f2e6db"
}
```
```

Only the **Name** field is required in the JSON file. Any number of distinct codes can be entered in the dataColors field.