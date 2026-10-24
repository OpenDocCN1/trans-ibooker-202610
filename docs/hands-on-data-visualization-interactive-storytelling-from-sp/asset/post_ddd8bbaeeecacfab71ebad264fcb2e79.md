
# NOTE

We set up our repo using GitHub’s template feature to make it easier for users to create their own copies. If you’re trying to copy someone else’s GitHub repo and don’t see a Template button, then click the Fork button, which makes a copy a different way. Here’s the difference: Template allows you to make multiple copies of the same repo by giving them different names, while Fork allows you to create only one copy of a repo because it uses the same name as the original, and GitHub prevents you from creating two repos with the same name. If you need to create a second fork of a GitHub repo, go to “Create a New Repo and Upload Files on GitHub”.

5. Click on the *index.html* file to view the code. If this is the first time you’re looking at computer code, it may feel overwhelming, but relax! We’ve inserted several “code comments” to explain what’s happening. The first block tells web browsers which formatting to apply to the rest of the page of code. The second block instructs the browser to load the **Leaflet** code library, the open source software that constructs the interactive map. The third block describes where the map and title should be positioned on the screen. The good news is that you don’t need to touch any of those blocks of code, so leave them as-is. But you do want to modify a few lines.

![](img/216890131d46798dfe1cd5f886cc608c_279_0.png)

6. To edit the code, click on the pencil symbol in the upper-right corner (✎).

Let’s start by making one simple change to prove to everyone that you’re now editing your map, by modifying the map title, which appears in the HTML division tag block around lines 21 to 23.

+   7. In the line <div id="map-title">EDIT your map title</div>, type your new map title in place of the words EDIT your map title. Be careful not to erase the HTML tags that appear on both ends inside the < > symbols.

+   8. To save your edit, scroll to the bottom of the page and click the green “Commit changes” button.

# Commit changes

Update index.html

Add an optional extended description...

Commit directly to the master branch.

Create a new branch for this commit and start a pull request.

Commit changes
Cancel

In the language of coders, we “commit” our changes in the same way that most people “save” a document, and later you’ll see how GitHub tracks each code commit so that you can roll them back if needed. By default, GitHub inserts a short description of your commit as “Update index.html,” and you have the option to customize that description when you start making lots of commits to keep track of your work. Also, GitHub commits your changes directly to the default branch of your code, which we’ll explain later.

# TIP

If you wish to store your code on GitHub but need to scale up to a larger commercial-level web host, see freemium services such as Netlify. Netlify automatically detects any changes you push to your GitHub repository and deploys them to your online site.

Now let’s publish your edited map to the public web to see how it looks in a web browser. GitHub not only stores open source code, but its built-in GitHub Pages feature allows you to host a live online version of your HTML-based code, which anyone with the web address can view in their browser. While GitHub Pages is free to use, there are some restrictions on usage, file size, and content, and it’s not intended for running an online business or commercial transactions. But one advantage of code templates is that you can host them on any web server you control. Since we’re already using GitHub to store and edit our code template, it’s easy to turn on GitHub Pages to host it online.

1. To access GitHub Pages, scroll to the top of your repo page and click Settings.

2. In the Settings screen, scroll way down to the GitHub Pages area. In the drop-down menu, change Source from None to Main, keep the default /(root) option in the middle, and press Save. This step tells GitHub to publish a live version of your map on the public web, where anyone can access it in their browser, if they have the web address.

# GitHub Pages

GitHub Pages is designed to host your personal, organization

# Source

GitHub Pages is currently disabled. Select a source below

Branch: main

/ (root)

Save

# Select branch

Select branch

main

None

# TIP

In response to the Black Lives Matter movement in 2020, GitHub renamed its default branch from master to main to eliminate its master-slave metaphor.

3. Scroll down to Settings > GitHub Pages to see the web address where your live map has been published online, and right-click it to open in a new browser tab.

# GitHub Pages

GitHub Pages is designed to host your personal, organization, or project pages from a GitHub repository.

![](img/216890131d46798dfe1cd5f886cc608c_282_0.png)

Now you should have at least two tabs open in your browser. The first tab contains your GitHub repo, where you edit your code, with a web address in the following format. Replace USERNAME and REPOSITORY with your own:

```
https://github.com/USERNAME/REPOSITORY
```

The second tab contains your GitHub Pages live website, where your edited code appears online. GitHub Pages automatically generates a public web address in this format:

```
https://USERNAME.github.io/REPOSITORY
```

# NOTE

The live version of your code points to the index.html page by default, so it’s not necessary to include it in the web address.

Remember how we advised you not to create your account with a username like DrunkBrownieChef6789? GitHub automatically places your username in the public web address.

Keep both tabs open so you can easily go back and forth between editing your code and viewing the live results online.

# TIP

GitHub Pages usually displays your live map in less than 30 seconds, but may require several minutes in some cases. If you see no change after one minute, give your browser a "hard refresh" to bypass any saved content in your cache and re-download the entire web page from the server using one of these key combinations:

+   - Ctrl+F5 (most browsers for Windows or Linux)
+   - Command-Shift-R (Chrome or Firefox for Mac)
+   - Shift-Reload button toolbar (Safari for Mac)
+   - Ctrl+Shift+Backspace (on Chromebook)

Now let’s edit your the GitHub repo so that the link points to *your* live map, instead of *our* live map:

+   1. Copy the web address of your live map from your second browser tab.
+   2. Go back to your first browser tab with your GitHub repo, and click on the repo title to return to its home page.
+   3. On your repo page, click to open the README.md file, and click the pencil again to edit it. Paste your live web link under the label (*replace with link to your site*) and scroll down to commit the change.

![](img/216890131d46798dfe1cd5f886cc608c_283_0.png)

![](img/216890131d46798dfe1cd5f886cc608c_284_0.png)

# # leaflet-map-simple

A simple leaflet map template for new users to fork their own copy, edit, and host on GitHub Pages

# ## Link to live map (replace with link to your site)

[https://handsondataviz.github.io/leaflet-map-simple/](https://handsondataviz.github.io/leaflet-map-simple/)

Now that you’ve successfully made simple edits and published your live map, let’s make more edits to jazz it up and help you learn more about how Leaflet code works.

+   4. On your repo home page, click to open the `index.html` file, and click the pencil symbol to edit more code.

Wherever you see the EDIT code comment, this points out a line that you can easily modify. For example, look for the code block shown below that sets up the initial center point of the map and its zoom level. Insert a new latitude

and longitude coordinate to set a new center point. To find coordinates, right-click on any point in Google Maps and select “What’s here?” as described in “Geocode Addresses in Google Sheets”.

```
var map = L.map('map', {
    center: [41.77, -72.69], // EDIT coordinates to recenter map
    zoom: 12,  // EDIT from 1 (zoomed out) to 18 (zoomed in)
    scrollWheelZoom: false,
    tap: false
});
```

The next code block displays the basemap tile layer that serves as the map background. Our template uses a light map with all labels, publicly provided by CARTO, with credit to OpenStreetMap. One simple edit is to change `light_all` to `dark_all`, which will substitute a different CARTO basemap with inverted coloring. Or preview several other Leaflet basemap code options that you can copy and paste. Make sure to attribute the source, and also keep `}) .addTo(map);` at the end of this code block, which displays the basemap:

```
L.tileLayer(
    'https://{s}.basemaps.cartocdn.com/light_all/{z}/{x}/{y}{r}.png', {
    attribution: '&copy; <a href="https://osm.org/copyright">\
OpenStreetMap</a> contributors, &copy;\
<a href="https://carto.com/attribution">CARTO</a>'
    }).addTo(map);
```

The last code block displays a single point marker on the map, colored blue by default in Leaflet, with a pop-up message when users click it. You can edit the marker coordinates, insert the pop-up text, or copy and paste the code block to create a second marker:

```
L.marker([41.77, -72.69]).addTo(map) // EDIT marker coordinates
    .bindPopup("Insert pop-up text here"); // EDIT pop-up text message
```

> **WARNING**
> 
> Be careful when editing your code. Accidentally removing or adding extra punctuation (such as quotation marks, commas, or semicolons) can stop your map from working. But breaking your code—and fixing it—can also be a great way to learn.

+   5. After making edits, remember to scroll down and press Commit to save changes. Then go to your browser tab with the live map, and do a hard refresh to view changes. Edits to your map normally will appear within 30 seconds, but remember that GitHub Pages sometimes requires longer to process code commits. If you have problems, see the Appendix.

Congratulations! If this is the first time that you’ve edited computer code and hosted it online, you can now call yourself a “coder.” The process is similar to following and modifying a cookbook recipe, just like you also can call yourself a “chef” after baking your first batch of brownies! Although no one is likely to hire you as a full-time paid coder (or chef) at this early stage, you now understand several of the basic skills needed to copy, edit, and host code online, and you’re ready to dive into the more advanced versions, such as Chart.js and Highcharts templates in Chapter 11 and Leaflet map templates in Chapter 12.

# Convert GitHub Pages Link to Iframe

In Chapter 9, we discussed the benefits of displaying interactive content from a primary site and making it appear seamlessly in a secondary site. You also learned how to convert very long Datawrapper and Tableau Public embed codes into shorter iframe tags when needed, so that you can embed them more easily on a secondary website.

The same concept applies to GitHub Pages. When you publish a code template for a chart or map (or any content) on GitHub Pages, it generates an online link that you can convert into an iframe tag, using the same principles as mentioned, to embed it on a secondary website. Follow these steps:

+   1. For any GitHub repository you have published online, go to its *Settings* page and scroll down to copy its GitHub Pages web address, which will appear in this general format:

```
https://USERNAME.github.io/REPOSITORY
```
