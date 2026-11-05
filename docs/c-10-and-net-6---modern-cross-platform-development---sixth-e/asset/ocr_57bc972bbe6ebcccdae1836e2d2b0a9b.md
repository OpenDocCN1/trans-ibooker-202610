13. Use Visual Studio Server Explorer or SQLiteStudio to connect to the Academy database and view the tables, as shown in Figure 10.6:

![](bbox=[0.17, 0.095, 0.93, 0.38])

Figure 10.6: Viewing the Academy database in SQL Server using Visual Studio 2022 Server Explorer

# Understanding migrations

After publishing a project that uses a database, it is likely that you will later need to change your entity data model and therefore the database structure. At that point, you should not use the Ensure methods. Instead, you need to use a system that allows you to incrementally update the database schema while preserving any existing data in the database. EF Core migrations are that system.

Migrations get complex fast, so are beyond the scope of this book. You can read about them at the following link: https://docs.microsoft.com/en-us/ef/core/managing-schemas/migrations/