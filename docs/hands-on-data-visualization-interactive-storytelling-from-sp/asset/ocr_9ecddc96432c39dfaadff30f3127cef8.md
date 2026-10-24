+   2. Convert it into an iframe by enclosing the link inside quotation marks as the source, and adding both start and end tags, in this general format:

```
<iframe src="https://USERNAME.github.io/REPOSITORY"></iframe>
```

+   3. If desired, improve the iframe appearance on the secondary site by adding any of these optional attributes, such as `width` or `height` (measured in pixels by default, or percentages), or `frameborder="0"` or `scrolling="no"`, in this general format:

```
<iframe src="https://USERNAME.github.io/REPOSITORY" width="100%"
height="400" frameborder="0" scrolling="no"></iframe>
```

> **TIP**
> 
> Either single quote (`'`) marks or double quote (`"`) marks are acceptable in your iframe code, but be consistent and avoid accidentally pasting in curly quotes (`“”`).

Now you are ready to paste your iframe into your preferred website (“Paste Code or iframe to a Website”), using methods described in Chapter 9, to display your interactive chart or map template from a published repository using GitHub Pages.

You should have a better sense of how to edit and host code repositories on GitHub. The next section describes how to enhance your GitHub skills by creating new repos and uploading your files. These are essential steps to create a second copy of a code template or to work with more advanced templates in the next two chapters.

# Create a New Repo and Upload Files on GitHub

Now that you’ve made a copy of our GitHub template, the next step is to learn how to create a brand-new repo and upload files. These skills will be helpful for several scenarios. First, if you have to fork a repo, which GitHub allows you to do only one time, this method will allow you to create additional copies. Second, you’ll need to upload some of your own files when creating data visualizations using Chart.js and Highcharts templates in Chapter 11 and Leaflet map templates in Chapter 12. Once again, we’ll demonstrate how to do all of these steps in GitHub’s beginner-level browser interface, but see “GitHub Desktop and Atom Text Editor to Code Efficiently” for an intermediate-level interface that’s more efficient for working with code templates.

In the previous section, you created a copy of our GitHub repo with the “Use this template” button, and we intentionally set up our repos with this newer feature because it allows the user to make *multiple* copies and assign each one a different name. Many other GitHub repos do not include a Template button, so to copy those you’ll need to click the Fork button, which automatically generates a copy with the same repo name as the original. But what if you wish to fork someone’s repo a second time? GitHub prevents you from creating a second fork to avoid violating one of its important rules: every repo in your account must have a unique name, to avoid overwriting and erasing your work.

How do you make a second fork of a GitHub repo if there’s no “Use this template” button? Follow our recommended workaround that’s summarized in the following three steps:

+   1. Download the existing GitHub repo to your local computer.
+   2. Create a brand-new GitHub repo with a new name.
+   3. Upload the existing code repo files to your brand-new repo.

Now let’s get into more detail about this three-step workaround:

+   1. Click on the Code > Download ZIP drop-down menu button on any repo. Your browser will download a zipped compressed folder with the contents of the repo to your local computer, and it may ask you where you wish to save it. Decide on a location and click OK.