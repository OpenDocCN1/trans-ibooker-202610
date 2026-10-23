Click the Save button and then click the Run button. If you experience any issues, please log an issue on the GitHub site where you downloaded this YAML file.

+   5. As with Exercise 8.5 and Exercise 8.6, you must additionally perform Exercises 7.1 (configuring a managed identity) and 7.2 (configuring EasyAuth) to get the application to render as expected. Refer to Figure 8.31, which illustrates the managed identity, key vault access policy, and EasyAuth configuration requirements.

+   6. To prove CI/CD is working, by default make a change to line 33 of the index.cshtml so that it modifies the following text color to red.

```
<h5 style="color: red">A JWT Token Does Not Exist</h5>
```

+   7. Take a look at Figure 8.37 that illustrates the following instructions. Save the index.cshtml file. Notice the check mark next to the file name. Click the Team Explorer tab, click the Changes option, add a comment (for example, Made H5 red), and then click the Commit All button. Click the Home button located toward the top of the Team Explorer tab; then click the Sync button. Now, click the Push link under the Outgoing Commits heading and monitor the Pipeline Runs tab. Once the Run completes, refresh the App Service page in the browser to confirm the update was published automatically.

# FIGURE 8.37 Azure DevOps Pipeline CI/CD process flow

![](bbox=[0.132, 0.475, 0.85, 0.853])