If that doesn’t work, go back to step 3 and experiment. Try to edit the embed code down to a *simple* iframe, and run it again to see how it looks. Sometimes a simple iframe works better on your website than a complex embed code.

```
```html
<!DOCTYPE html>
<html>
<body>

<h1>The iframe element</h1>

<iframe src="https://datawrapper.dwcdn.net/r2bJ/" scrolling="no" frameborder="0" width="100%" height="400"></iframe>

</body>
</html>
```
```

![](bbox=[0.665, 0.128, 0.902, 0.598])

# TIP

The Datawrapper iframe tag source follows this general format: `https://datawrapper.dwcdn.net/abcdef/1/`, where the 1 refers to the first version of the chart or map you published. If you make edits and re-publish your visualization, Datawrapper will increase the last digit (to 2 and so forth), and automatically redirect older links to the current version, which keeps your work up to date for your audience.

# From Tableau Public

After you create a Tableau Public chart in “Tableau Public Charts” or map in “Choropleth Map with Tableau Public”:

+   1. Publish your worksheet, dashboard, or story online by selecting File > “Save to Tableau Public” in the desktop application menu.
+   
+   2. In your online Tableau Public account profile page, click View to see the details of any of your published visualizations.