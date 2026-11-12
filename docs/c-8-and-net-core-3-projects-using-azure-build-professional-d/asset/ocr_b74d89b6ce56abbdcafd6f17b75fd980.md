```
Database=WebResearch;
Trusted_Connection=True"
}
}
```

Later on, we will look at how to connect to an existing database, but for now, we are just connecting to a local db file.

# # Creating the database

During the development phase of any application, there is a fairly high probability that your data model will change. When that happens, your EF Core model differs from the database schema and you have to delete the outdated database and create a new one based on the updated model.

This is all fun and games until you've done your first live implementation and your application runs in a production environment. You cannot go and drop a database just to change a few columns. You have to make sure the live data persists when you make any changes.

EF Core migrations is a nifty feature that allows us to make changes to the database schema instead of recreating the database and losing production data. There is a lot of functionality and flexibility that's possible with migrations, and it is a topic well worth spending time on, but we'll just cover some of the basics for now.

We can use EF Core Migration commands in the Package Manager Console to set up, create, and, if needed, update our database.

In the Package Manager Console, we will execute the following two commands:

+   • Add-Migration InitialCreate
+   • Update-Database

The first command will generate the code in the `Migrations` folder of the project, which is used to create the database. The naming convention for these files is `<timestamp>_InitialCreate.cs`.