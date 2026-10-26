
3. The data preview shows how the wizard has interpreted the rows and columns; if this analysis is incorrect, the schema of the new target table can be altered in the next step of the wizard, as shown in the following screenshot:

![](img/07d1225e179d5df9f2b2d26b026386c6_54_0.png)

Import Flat File—Modify Columns

Starting a vulnerability scan is simple. Right-click on a desired database, choose the Task menu, and navigate to Vulnerability Scan. The options available are to run a new scan or open the report from a previous scan. A scan that is run will store the results in a user-defined location, but it defaults to the user's default documents location. The following screenshot illustrates the process:

![](img/07d1225e179d5df9f2b2d26b026386c6_56_0.png)

Once a scan is completed, SSMS will automatically open the generated report and display it inside SSMS. At the time of writing, it is not possible to automatically export the results in a different format (for example, Word or Excel). The basis of the report is a JSON file, so any further processing would require waiting for an export functionality from Microsoft or require your ingenuity in parsing the JSON file.

The resulting report displayed in SSMS allows for further analysis of the security threats that may have been found. In the following screenshot, we can see an example scan of an AdventureWorks database:

![](img/07d1225e179d5df9f2b2d26b026386c6_57_0.png)

Vulnerability scan results

The scan indicates that the database has passed 50 tests and failed 4. By clicking on one of the tests we can see more details about the failure/pass.

The following screenshot shows the details of the failed check ID VA1219, which states that a database should have Transparent data encryption activated:

![](img/07d1225e179d5df9f2b2d26b026386c6_58_0.png)

Vulnerability scan—failed TDE

Each check is accompanied with a range of information, including the query that was run against the scanned database. This allows us to see whether the check is correct for our database/environment.

Should a certain check be irrelevant for a database/environment, the Approve as Baseline button can be clicked. This will override the check outcome to ensure that a future scan against this database will deem the overridden check to be a pass rather than a failure.

Upon marking the failing TDE check as an acceptable value, a second scan of the AdventureWorks database provides us with 51 passes and 3 failures. The TDE check is now listed as a passing check, with the extra information being set as a custom baseline value. The following screenshot illustrates the baseline:

![](img/07d1225e179d5df9f2b2d26b026386c6_59_0.png)

The Vulnerability Assessment is by no means a perfect tool, currently *only covering* a limited set of scenarios. It does, however, provide a good start to assessing the security threats inside a database. The tool will continue to receive updates (especially to the rules that are checked against). A further tool to help improve the security of database systems should always be welcomed.

## SQL Server Data Tools

As with the installation of SSMS, SQL Server Data Tools is also offered as a separate download. This can be found using the SQL Server setup screen shown at the beginning of the chapter. Clicking on **Install SQL Server Data Tools** will launch a web browser, directing you to the **Downloads** page for SSDT. This **Downloads** page offers the latest stable build and also the latest release candidate of the next version of SSDT (with the usual warning of a release candidate not being production ready). SSDT is delivered with the same Visual Studio Integrated Shell as SSMS, and can be installed as a standalone tool. However, SSDT is aimed at developers and the workflows associated with developing database solutions as a team member. This includes the processes of source control and the packaging of project deployments. With this in mind, it is also possible to install SSDT on a machine that has the full Visual Studio environment installed. Doing so will integrate SSDT into Visual Studio and incorporate the database development templates and workflows, allowing Visual Studio to remain in the development environment, instead of adding a separate environment just for database development.

If Visual Studio is already installed, the SSDTs update can be installed from inside Visual Studio. To download and install this update, navigate to Tools | Extensions and Updates and select the node Updates on the left side of the Extensions and Updates modal window, as shown in the following screenshot:

![](img/07d1225e179d5df9f2b2d26b026386c6_60_0.png)

SSDT—extensions and updates

Once installed, SSDT (whether installed as a standalone tool or integrated into Visual Studio) provides four separate project templates to help jump-start development of SQL Server projects:

- **Relational databases**: This is a template designed for traditional relational database development and it supports SQL Server versions 2005 through to 2016 (although SQL Server 2005 is now a deprecated version). SSDT also supports on-premises installations and also Azure SQL Database projects (the Database as a Service solution hosted in Microsoft Azure). It is also possible to design queries (but not full projects) for Azure SQL Data Warehouse (the cloud-based data warehouse solution, hosted in Microsoft Azure).
- **Analysis Services models**: This template is designed to assist in the design and deployment of Analysis Services projects and it supports SQL Server versions 2008 through to 2016.
- **Reporting Services reports:** This template is designed to assist in the design and deployment of Reporting Services projects and it supports SQL Server versions 2008 through to 2016.
- **Integration Services packages:** This template is designed to assist in the design and deployment of Integration Services projects and it supports SQL Server versions 2012 through to 2016.

The template choice dictates what files and folders are automatically prepared at project creation time. Choosing **File | New Project** presents the new project dialogue shown in the following screenshot. There are two additions to the project type navigation tree. **Business Intelligence** groups **Analysis Services**, **Reporting Services**, and **Integration Services** projects together. The relational database project type is found under the **SQL Server** navigation node, as shown in the following screenshot:

![](img/07d1225e179d5df9f2b2d26b026386c6_61_0.png)

SSDT—new project dialogue

As shown in the preceding screenshot, a project folder is created and the option to add the project to a source control system is available. Visual Studio offers the option to natively access the source control system's Visual Studio Team Services (a hosted source control system from Microsoft) or alternatively to use a local source control system such as Git. Furthermore, source control systems can be added through the extensive extensions library offered through Visual Studio and they can be accessed through the **Tools | Extensions and Updates** menu described earlier in this section.

Working with SSDT and the aforementioned templates should be familiar to any developer that has used Visual Studio before. Upon creating a project, the next step is to begin by adding new items (tables, view, stored procedures, and so on). The dialogue for this is filtered down to the project type, and for an SQL Server database project, we have the option of creating items ranging from Application Roles to XML Schema Collections. Of note is the ability to create **SQL CLR objects** (C# based common language runtime objects), which provide the ability to create more complex calculations that are otherwise not possible (or perform poorly) in the T-SQL language:

![](img/07d1225e179d5df9f2b2d26b026386c6_62_0.png)
