
8. This step simply checks to make sure that the latest version of SQL Server is installed. Click Next once the setup files are installed, as shown in the following screenshot:

![](img/07d1225e179d5df9f2b2d26b026386c6_317_0.png)

9. SQL Server setup will check several rules to ensure that the computer where we want to install it is setup properly. You might get a warning due to Windows firewall rules. This tells you that the port (1433 by default) is not open and SQL Server won’t be available from outside your PC. Don’t worry about it. Since we’ll be using SQL Server from our PC only, we do not need to open any ports for now. Click Next to advance to the feature selection page, as shown in the following screenshot:

![](img/07d1225e179d5df9f2b2d26b026386c6_318_0.png)

10. Select all features checked in the preceding screenshot and click Next, as shown in the following screenshot:

![](img/07d1225e179d5df9f2b2d26b026386c6_319_0.png)

- 11. Instance configuration allows to specify a name for the SQL Server service. This is done by selecting the **Named instance** radio button. Since we'll only use one SQL Server instance, leave **Default instance** selected and click **Next**, as shown in the following screenshot:

![](img/07d1225e179d5df9f2b2d26b026386c6_320_0.png)

12. This page allows SQL Server to be part of a PolyBase scale out group. Since we're only setting up SQL Server PolyBase to be used by one instance, leave the default **Use this SQL Server as standalone PolyBase-enabled instance** and click **Next**, as shown in the following screenshot:

![](img/07d1225e179d5df9f2b2d26b026386c6_321_0.png)

13. Now for server configuration. This step allows us to specify distinct or specific service accounts. Since we're installing SQL Server on a single development machine, we'll use the default accounts, as shown in the following screenshot:

![](img/07d1225e179d5df9f2b2d26b026386c6_322_0.png)

14. Click on the Collation tab as highlighted on the preceding screenshot. The default collation used by SQL server is SQL_Latin1_General_CP1_CI_AS. This is a legacy collation. The choice of the collation is important for character string columns. The latest (fewer bugs) collation is Latin1_General_100. The last characters CI and AS are for case-insensitive and accent-sensitive, respectively.

- 17. If we query SQL Server filtering on `FirstName = 'Joel'` with the **Accent-sensitive** collation option, we end up retrieving the value Joel only. If we do not select the **Accent-sensitive** collation option, we will get both values.

- 18. Click OK when done to return to the previous screen. We'll do the same for analysis services; click **Customize...** to customize analysis service collation, as shown in the following screenshot:

![](img/07d1225e179d5df9f2b2d26b026386c6_324_0.png)

19. Again, choose `Latin1_General_100` in Collation designator and uncheck the **Accent-sensitive** checkbox. Click **OK** to return to the previous screen. Click on **Next**, as shown in the following screenshot:

![](img/07d1225e179d5df9f2b2d26b026386c6_325_0.png)

20. This will direct you to the following screen. For the database engine configuration, we'll use Mixed Mode to allow us to use SQL Server logins and Windows logins. The default authentication is Windows authentication mode, which is more secure than SQL Server authentication because it uses the Kerberos security protocol, password, and account lockout policies, and password expiration. Make sure you use strong passwords for SQL Server logins. By default, password policy, password expiration, and user must change password at next login are turned on also for SQL Server login. You should not disable the password policy and the password expiration. Select the Mixed Mode radio box and enter a password for the SA account. Click on Add Current User as shown in the screenshot to add your Windows account as an administrator of the instance. You'll have all rights on it. Click Next, which will direct you to the Analysis Services Configuration window, as shown in the following screenshot:

![](img/07d1225e179d5df9f2b2d26b026386c6_326_0.png)

21. For analysis services configuration, the Server Mode we'll use is Tabular Mode and again click on Add Current User as shown in the following screenshot to add your Windows account as an administrator of the service. Click Next.

22. For the Reporting services configuration, leave the default values and click Next, as shown in the following screenshot:

![](img/07d1225e179d5df9f2b2d26b026386c6_328_0.png)

23. We're finally ready to install. Click **Install** to start the installation process, as shown in the following screenshot:

![](img/07d1225e179d5df9f2b2d26b026386c6_329_0.png)

24. The following screenshot shows the installation progress:

![](img/07d1225e179d5df9f2b2d26b026386c6_330_0.png)

25. Once the installation is complete, you get the following screen:

![](img/07d1225e179d5df9f2b2d26b026386c6_331_0.png)

We're done. We just installed SQL Server 2016! In the next section, we'll install SQL Server Management Studio.

> The number, the order, and the appearance of the setup screens change slightly with every version of SQL Server, or even with a service pack. If you encounter a new screen not mentioned here, just use the default settings and proceed with the installation.

## SQL Server Management Studio installation

SQL Server Management Studio is a separate download from SQL Server. This program will allow us, among other things, to create database objects and query SQL Server. Without this tool, we wouldn't be able to manage SQL Server databases easily.

## Getting ready

This section assumes that you have installed SQL Server 2016.

## How to do it...

- 1. To download and install SQL Server Management Studio, click on Install SQL Server Management Tools, as shown in the following screenshot:

![](img/07d1225e179d5df9f2b2d26b026386c6_332_0.png)

- 4. This will direct you to the Microsoft SQL Server Management Studio installation screen as follows. The installation is in progress; it may take several minutes to complete.

![](img/07d1225e179d5df9f2b2d26b026386c6_335_0.png)

5. Click **Close** to close the installation wizard, as shown in the following screenshot:

![](img/07d1225e179d5df9f2b2d26b026386c6_336_0.png)

## SQL Server Data Tools installation

The last part of our SQL Server 2016 setup is to install SQL Server Data Tools. This will install a Visual Studio Shell that contains BI templates necessary for the following:

- SQL Server integration services
- SQL Server analysis services

## Getting ready

We'll use SSDT throughout this book to create, deploy, and maintain our SSIS packages and some databases.

## How to do it...

- 1. From the SQL Server 2016 setup utility, click on SQL Server Data Tools (SSDT). This will open the Download SQL Server Data Tools (SSDT) download page in your browser as shown in the following screenshot:

![](img/07d1225e179d5df9f2b2d26b026386c6_337_0.png)

Here, there are two choices:

Install SSDT only: This is the simplest scenario. It only installs SSDT and a development shell.

**Install Visual Studio and SSDT:** You choose this if you plan to use source control inside Visual Studio or when you want to implement different types of development (.NET, Python, and so on) such as SSIS/SSAS/SSRS development. Since we'll talk about custom components in this book, we'll install Visual Studio Community Edition. This version is free for individuals.

- 2. Click on the **Download Visual Studio Community 2015** link to download the Visual Studio installer.
- 3. Once downloaded, click on **Run** to launch the Visual Studio installer.
- 4. Accept the default installation type and click **Install** to start the installation process. This will give you the following window:

![](img/07d1225e179d5df9f2b2d26b026386c6_338_0.png)
