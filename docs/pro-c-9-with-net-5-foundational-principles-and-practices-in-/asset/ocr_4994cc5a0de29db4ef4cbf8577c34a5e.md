Note It is extremely important that you don’t delete migration files manually. This will result in the <DerivedDbContext>ModelSnapshot.cs becoming out of sync with your migrations, essentially breaking them. If you are going to manually delete them, delete them all and start over. To remove a migration, use the remove command, covered shortly.

# Excluding Tables from Migrations

If an entity is shared between multiple DbContexts, each DbContext will create code in the migration files for any changes to that entity. This causes a problem since the second migration script will fail if the changes are already present in the database. Prior to EF Core 5, the only solution was to manually edit one of the migration files to remove those changes.

In EF Core 5, a DbContext can mark an entity as excluded from migrations, letting the other DbContext become the system of record for that entity. The following code shows an entity being excluded from migrations:

```
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<LogEntry>().ToTable("Logs", t => t.ExcludeFromMigrations());
}
```

# The Remove Command

The remove command is used to remove migrations from the project and always operates on the last migration (based on the timestamps of the migrations). When removing a migration, EF Core will make sure it hasn’t been applied by checking the __EFMigrationsHistory table in the database. If the migration has been applied, the process fails. If the migration hasn’t yet been applied or has been rolled back, the migration is removed, and the model snapshot file is updated.

The remove command doesn’t take any arguments (since it always works on the last migration) and uses the same options as the add command. There is one additional option, the force option (-f || --force). This will roll back the last migration and then remove it in one step.

# The List Command

The list command is used to show all of the migrations for a derived DbContext. By default, it will list all migrations and query the database to determine whether they have been applied. If they have not been applied, they will be listed as pending. There is an option to pass in a specific connection string and another option to not connect to the database at all and instead just list the migrations. Table 22-12 shows those options.

Table 22-12. Additional Options for EF Core Migrations List Command

| Option (Shorthand || Longhand) | Meaning in Life |
|---|---|
| --connection <CONNECTION> | Connection string to the database. Defaults to the one specified in the instance of IDesignTimeDbContextFactory or the DbContext’s OnConfiguring method. |
| --no-connect | Instructs the command to skip the database check. |