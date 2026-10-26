13. Now for server configuration. This step allows us to specify distinct or specific service accounts. Since we're installing SQL Server on a single development machine, we'll use the default accounts, as shown in the following screenshot:

![](bbox=[0.105, 0.185, 0.895, 0.67])

14. Click on the Collation tab as highlighted on the preceding screenshot. The default collation used by SQL server is SQL_Latin1_General_CP1_CI_AS. This is a legacy collation. The choice of the collation is important for character string columns. The latest (fewer bugs) collation is Latin1_General_100. The last characters CI and AS are for case-insensitive and accent-sensitive, respectively.