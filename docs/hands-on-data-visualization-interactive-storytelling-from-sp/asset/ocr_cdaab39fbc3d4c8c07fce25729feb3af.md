10. In your Leaflet map code on your GitHub repo, open the google-doc-url.js file, click the pencil symbol to edit it, and paste in your Google Sheets API key to replace our key. Be careful not to erase the single quote marks or the semicolon. Scroll down to Commit your changes.

![](bbox=[0.22, 0.09, 0.9, 0.3])

You might receive a notification from GitHub stating that you have an exposed API key, but don’t worry. This key can be used only with Google Sheets, you received it for free, and you didn’t attach any billing information to it, so Google cannot charge you for its use.

Now that you’ve learned how to create a Google Sheets API key to use with Leaflet Maps with Google Sheets or Leaflet Storymaps with Google Sheets, in the next sections you’ll learn more about other types of Leaflet map templates.

# Leaflet Maps with CSV Data

This open source template is designed to improve your coding skills by demonstrating how to create a Leaflet point map that pulls data from a CSV file located in your GitHub repo. While you can make the same type of map on other platforms, such as Google My Maps as described in “Point Map with Google My Maps”, you’ll learn more about how the Leaflet code library works by doing it yourself.

Figure 12-7 shows a simple point map of some colleges and universities in Connecticut. Instead of individually creating markers in JavaScript using Leaflet’s L.marker() function, the point data is stored in a local CSV file (data.csv) that’s easy to modify in any text editor or spreadsheet. Each time the map is loaded by the browser, point data from the CSV file is read and markers are generated “on the fly.”