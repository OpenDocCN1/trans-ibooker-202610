
![](img/8e59e0a64e4cc0c26d8e7f463c62aaad_418_0.png)

Figure 8-18. What we see at the top of the page if we open a Power BI project file

Clicking the small arrow to the right of the parenthetical note reveals additional details about the file, including the locations of the report and the semantic model (see Figure 8-19). Clicking any of the file paths directly opens the folder location.

![](img/8e59e0a64e4cc0c26d8e7f463c62aaad_418_1.png)

Figure 8-19. Clicking the project name to open a pop-up with file and folder details

## Benefits of Using Power BI Project Files

So why use Power BI project files? One of the main benefits is that they let you make changes outside of Power BI Desktop. Because the report and semantic model are stored separately, you can use tools like Visual Studio Code (VS Code) to edit the text definitions directly. This is particularly useful when you’re updating multiple measures or making widespread changes, because you won’t have to click through all items individually in Power BI Desktop.

Another benefit is that the folder structure makes version control easier. When everything is in one .pbix file, tracking small changes in Git is not really possible. But with Power BI projects, the files are split, and that makes it easier for you to see what exactly changed in a commit: the model, the report, or both.

Finally, using project files makes reuse more straightforward. You can copy parts of the project, like a report page or model object, into another project without having to rebuild it from scratch.

It’s also important to note that using project files doesn’t change the end-user experience. A report saved as a .pbix file and another saved as a .pbip file will look and behave the same in Power BI Desktop and the service. The difference lies in how the files are stored, managed, and maintained, which is especially relevant for developers.

## Limitations of Power BI Project Files

You also need to be aware of several practical limitations when you’re working with Power BI project files. Power BI Desktop does not automatically detect changes made with external tools, so any edits you perform outside of Desktop will only appear after you restart the application. Features like sensitivity labels and report page synonyms are not supported, and that can affect governance and metadata management. The .pbip files also have a maximum path length of 260 characters by default, so you need to use short folder paths to avoid save errors. Additionally, you can’t save .pbip files directly to OneDrive or SharePoint; doing so can create syncing problems and potentially cause failed save operations.

## Reusable Assets in Power BI

When working with Power BI, we often create multiple reports, build different semantic models, and set up data connections repeatedly. Many of these don’t need to be rebuilt each time, and we also want to avoid starting from scratch every single time.

A better approach involves using *reusable assets*, which are files or objects that capture parts of our work, such as report templates, data source definitions, and existing semantic models. We can reuse these assets in other projects, and that not only saves us time but also maintains consistency across teams and environments.

In this section, we’ll explore three types of reusable assets: Power BI template files (.pbit), Power BI data source files (.pbids), and shared semantic models. Each one serves a different purpose, but all of them make developing in Power BI more efficient and organized.

## Power BI Template Files

In Power BI Desktop, we can save a report as a template file. A *template* captures the structure of a report, allowing us or others in our organization to use it as a starting point for new reports. This is helpful when we need to use the same layout, model, or queries repeatedly. When we save a Power BI report as a Power BI template, we get a single file (.pbit) that includes the following parts of our report:

- Report pages, visuals, and other visual elements
- The semantic model definition, including schema, relationships, and measures
- Power Query definitions (including parameters)

A template contains the structure of the report (pages, visuals, model, and queries), but not the data itself. When we open a Power BI template file, Power BI asks us to connect to the data source to load data. Templates are therefore useful for reuse and standardization.

## Creating a Power BI Template File

To create a template file from a Power BI report, you simply save it as a .pbit file instead of a .pbix file. This doesn’t overwrite the .pbix file, but it creates a new file. Figure 8-20 shows the menu option you see when saving the report.

![](img/8e59e0a64e4cc0c26d8e7f463c62aaad_421_0.png)

Figure 8-20. The Power BI option for saving a template file

Then, you click Save, and Power BI will bring up a screen where you can write a description of the template before the system saves it (see Figure 8-21). As always, we recommend that you write a clear description, for the sake of both your coworkers and yourself, since it might be a year or more before you look at the file again.

# Export a template

Template description

Monthly operations report template

Figure 8-21. Screen where you can write a description of a template file before it’s saved

## Using Report Templates

To use a Power BI report template, you can open the file in Power BI Desktop either by launching Power BI Desktop itself or by double-clicking on the file. If any parameters were used in the Power BI file that you saved as a Power BI template, the first message you’ll see will direct you to fill in the parameter value (see Figure 8-22).

# Operations Analytics

Country

Figure 8-22. Screen where you can fill in parameter values if any parameters were used in the source file

After you fill in the parameter, Power BI will ask for the location of the data source. At this point, you can connect to the data by using your own credentials. Figure 8-23 shows a prompt for credentials for a web source.

![](img/8e59e0a64e4cc0c26d8e7f463c62aaad_423_0.png)

Figure 8-23. Filling in your own user credentials when opening a file and connecting to a Power BI template

> **NOTE**
> You should know that when you try this with your own file, Power BI may not ask for your credentials if the original data source connection permission is still present. To test this whole process, remove the data source permission from the source file and then export the template. After that, Power BI will prompt you to enter credentials when you open the template.

Using report templates is an easy way to reuse report structures without copying the data. It saves time, helps maintain consistency, and simplifies sharing a standard starting point with others. In our daily work, we primarily use .pbix files, but .pbit templates are handy when we need to use the same layout, model, or queries repeatedly.

## Power BI Data Source (.pbids) Files

Power BI template files are very useful for reusing the same report layout and semantic model relationships, but sometimes, we only want to reuse the connection from a Power BI file. We can do this by saving and then importing the data source connection as a Power BI Data source file (.pbids).

In a .pbids file, we don’t have any visuals like in a Power BI template file; a .pbids file is a JSON text file that holds the data source connection. It doesn’t contain data or queries, just the following connection information:

- The type of data source (for example, Excel or SharePoint)
- The connection details (such as a database or file path)
- Optional authentication settings, depending on the data source

Here’s an example of what a .pbids file looks like:

```
{
  "version": "0.1",
  "connections": [
    {
      "details": {
        "protocol": "sharepoint-list",
        "address": {
          "url": "https://[url]"
        },
      }
    }
  ]
}
```
