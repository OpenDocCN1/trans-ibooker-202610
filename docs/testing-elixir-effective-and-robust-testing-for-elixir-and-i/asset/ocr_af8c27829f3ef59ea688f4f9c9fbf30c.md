# The database for TestingEcto.Repo has been created

Whenever you’re having your application make changes to your database structure, it’s good practice to check that the expected changes are reflected in the database. In the second shell session, run `make cli` to connect to the Postgres cli client inside your Docker container. It verifies that your database was created correctly if you’re able to connect using this command. Since we’ve only created the database, there isn’t much for us to do now except to run `\l` to list the available databases. `testing_ecto_test` should be on the list. Now we need to add a table to the database to correlate with the schema that we’re setting up. Back at the command line in the application directory, run `mix ecto.gen.migration add_users_table`. This will generate the nested directories and a file with a timestamp in the name, like in the following output (you’ll have a different timestamp):

```
Generated testing_ecto app
* creating priv/repo/migrations
* creating priv/repo/migrations/20200518212741_add_users_table.exs
```

Open the file and you should see a migration file with an empty `change/0` function. Let’s add code to create a table to match our schema. Inside the `change/0` function, add the code to make your file match the following:

```
testing_ecto/priv/repo/migrations/20200518212741_add_users_table.exs

1: defmodule TestingEcto.Repo.Migrations.AddUsersTable do
   use Ecto.Migration
   def change do
5:   create table(:users, primary_key: false) do
      add(:id, :uuid, primary_key: true)
      add(:date_of_birth, :date, null: false)
      add(:email, :string, null: false)
      add(:favorite_number, :float)
      add(:first_name, :string, null: false)
10:
```