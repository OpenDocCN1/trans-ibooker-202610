fillOpacity:0.9. You can assign colors with standard names, hex codes, or RGBa values as described in the W3Schools Colors Picker.

Inside the template you’ll discover more ways to customize your storymap, such as:

+   - Insert logo (see Options tab in Google Sheets)

+   - Insert Google Analytics tracking ID (see Options tab in Google Sheets)

+   - Adjust title size and font (go to css/styles.css file in GitHub)

+   - Insert a horizontal divider in Chapter text (copy and paste this text into Description field in Google Sheets, and avoid changing single quote marks into curly apostrophes):

```
<span style='display:block;width:100%;height:1px;background-color:
silver; margin: 20px 0;'></span>
```

Now you’re ready to finalize your map. If you wish to share your map link with the public, read the options here and choose either step G or step H.

> WARNING
> We reserve the right to change our Google Sheets API key at any time, especially if other people overuse or abuse it. This means that you must finalize your map using either step G or H before sharing it publicly because it will stop working if we change our key.

# G. Save each Google Sheets tab as a CSV file and upload to GitHub

If you have finished entering most of your data into your Google Sheets, downloading it into separate CSV files and uploading those into your GitHub repo is the best long-term preservation strategy. This approach keeps your map and data together in the same GitHub repo, and removes the risk that your map will break due to an interruption to Google services. Plus, you can still edit your map data. If this approach makes sense, follow these steps:

+   1. In your Google Sheets, go to each tab and select File > Download into CSV format, to create a separate file for each tab.

+   2. Shorten each filename as shown. The names must be exact. The first two files are required, and others are optional:

+   - Chapters.csv

+   - Options.csv

+   - Notes.csv (or .txt) Recommended to keep any notes with your data, but not required.

+   3. In your GitHub repo, click the csv subfolder to open it, select “Add file” > “Upload files,” and upload all of the CSV files listed into this subfolder. The Leaflet template code checks here first for data, and if it finds CSV files with these names, it will pull the map data directly from them, instead of your Google Sheets. Remember that from this point forward, any edits in your Google Sheet will no longer appear automatically in your map.

![](bbox=[0.22, 0.68, 0.9, 0.86])

+   4. If you wish to edit your map after uploading your CSV files, you have two options. You can make small edits directly to your CSV files by opening them in the GitHub web interface, or you can make larger edits in the Google Sheet, repeating the previous steps to download them in CSV format and upload them to replace your existing files on GitHub.

# H. Get your own Google Sheets API Key to insert into the code