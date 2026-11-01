
```
add(:last_name, :string, null: false)
add(:phone_number, :string, null: false)
timestamps()
end

create(unique_index(:users, [:email]))
end
end
```

There are a couple of details to call out here. The first is that we’re telling the migration to skip giving the table an auto-incrementing primary_key at line 5 and then add a primary key on line 6. Our table will use UUIDs for the primary key. We’ll need to remember this when we update our schema later. We also used the null: false option on all of our columns except favorite_number, which we left optional on our schema. The last thing to note in our change/0 is that with the addition of timestamps() (line 14), we’ll be adding two fields that aren’t currently on our schema, inserted_at and updated_at. These will also factor into the updates to our schema.

Below the table definition, we also create a unique constraint on the email column (line 17). We’ll address this once we’ve started interacting with the database in our tests.

Now we need to run our migration to make sure that it works. We’re going to focus on the test database here, so make sure to set the correct mix environment by running MIX_ENV=test mix ecto.migrate. This should produce successful feedback:

```
15:36:12.729 [info] == Running 20200518212741
TestingEcto.Repo.Migrations.AddUsersTable.change/0 forward

15:36:12.730 [info] create table users
```

15:36:12.738 [info] == Migrated 20200518212741 in 0.0s

Go back to your session logged into the Docker container, where the Postgres cli is open. You should now be able to run \dt to see the tables users and schema_migrations. Run \d users and verify that you see the following table definitions:

Table "public.users"

| Column | Type | Collation | Nullable | Default |
|---|---|---|---|---|
| id | uuid |  | not null |  |
| date_of_birth | date |  | not null |  |
| email | character varying(255) |  | not null |  |
| favorite_number | double precision |  |  |  |
| first_name | character varying(255) |  | not null |  |
| last_name | character varying(255) |  | not null |  |
| phone_number | character varying(255) |  | not null |  |
| inserted_at | timestamp without time zone |  | not null |  |
| updated_at | timestamp without time zone |  | not null |  |

Indexes:
"users_pkey" PRIMARY KEY, btree (id)

Assuming your table matches ours, we’re ready to move on.

## Updating Our Schema to Work with the Database

We’re now ready to update our schema to match this table. Technically we’ll update a copy to the schema so that we can make changes without breaking our other test files. Copy testing_ecto/lib/schemas/user_basic_schema.ex to a new file called testing_ecto/lib/schemas/user_database_schema.ex. We’ll copy the test file to match later.

For now, we’re going to update the schema file to look like this:

```
testing_ecto/lib/schemas/user_database_schema.ex

1: defmodule TestingEcto.Schemas.UserDatabaseSchema do
  - use Ecto.Schema
```

```elixir
- import Ecto.Changeset
-
5: @timestamps_opts type: :utc_datetime_usec
@primary_key {:id, :binary_id, autogenerate: true}
@optional_fields [:id, :favorite_number]
-
schema "users" do
  field(:date_of_birth, :date)
10:
-
  field(:email, :string)
  field(:favorite_number, :float)
  field(:first_name, :string)
  field(:last_name, :string)
  field(:phone_number, :string)
15:
-
-
  timestamps()
-
  end
-
  defp all_fields do
20:
-
  __MODULE__.__schema__(:fields)
-
  end
-
  def changeset(params) do
    %__MODULE__{}
25:
-
    |> cast(params, all_fields())
    |> validate_required(all_fields() -- @optional_fields)
    |> unique_constraint(:email)
    end
  end
30:
```

First, change the module name in user_database_schema.ex to be TestingEcto.Schemas.UserDatabaseSchema, like on line 1. We need to set some Ecto Schema options in the file (using module attributes) to make

it compatible with the timestamp and primary key options we set in our migration. Add two options @timestamps_opts type: :utc_datetime_usec (line 5) and @primary_key {:id, :binary_id, autogenerate: true} (line 6) to the file. These are options that could be set in a common schema template if this were a real application. If you recall, our basic schema had @primary_key false set right above the schema definition. Now that it’s being updated to correspond to the database table, we’ll need to remove that. By removing that, and by setting the option for the primary key to be a :binary_id, our schema now has an additional field, :id, of type :binary_id.

For now, our changeset function is still a generic changeset function and therefore would presumably be used for creating a new user. As a result, while :id might be needed for an update changeset, we’ll need to make it an optional field for when it’s used for create. Later we’ll split this function into two more specific changeset functions, but we’re keeping a simple one for now to keep the examples smaller. Notice that we updated the module attribute with optional fields to include :id (line 7).

At this point, our schema isn’t designed to reference a database table because it uses embedded_schema and not schema. We can update that line of the definition to say schema "users" do (line 9) and the schema will now be set to reference our “users” table. We’ve strayed from the convention of having the module name match the table name because we’re dealing with a bunch of different versions of the same file. Normally, we would’ve named our schema “User” and the table “users.” At the bottom of the definition block, add timestamps(), like on line 17, to add :inserted_at and :updated_at to the schema.

We made the email field unique, so we need to update the changeset to take that into account, adding in the call to unique_constraint/2 on line 28. And that’s it. Our schema file is now set up to reference a database table. Next, we’ll update our tests to take the new fields into account. When that’s done, we’ll look at how to test the unique_constraint code.

## Updating Our Tests to Include Database Fields

We have set up our schema, UserDatabaseSchema, to correspond with a database table, but that means that there are three new fields (:id, :inserted_at, and :updated_at). To make these changes, we’ll make yet another copy of the last iteration of our basic schema test file, testing_ecto/test/schemas/user_basic_schema_3_test.exs. Make a copy of it, named testing_ecto/test/schemas/user_database_schema_test.exs. Let’s update the test name to TestingEcto.Schemas.UserDatabaseSchemaTest and also replace all of our references to UserBasicSchema with UserDatabaseSchema, including the alias.

With the correct module names, your test file should be able to run. Run mix test --only schema_definition test/schemas/user_database_schema_test.exs and you should see some errors, because we added new fields, but the test’s definition of the fields and types (stored in @expected_fields_with_types) hasn’t been updated. The test that we should be looking at will have this output:

```
1) test fields and types it has the correct fields and types
(TestingEcto.Schemas.UserDatabaseSchemaTest)
test/schemas/user_database_schema_test.exs:23
Assertion with == failed
code: assert Enum.sort(actual_fields_with_types) ==
    Enum.sort(@expected_fields_with_types)
left: [{:date_of_birth, :date}, {:favorite_number, :float},
    {:first_name, :string}, {:id, :binary_id},
    {:inserted_at, :utc_datetime_usec}, {:last_name, :string},
    {:phone_number, :string}, {:updated_at, :utc_datetime_usec}]
right: [date_of_birth: :date, favorite_number: :float,
    first_name: :string, last_name: :string, phone_number: :string]
stacktrace:
    test/schemas/user_database_schema_test.exs:30: (test)
```

We are familiar with this output and know what to do. We need to add :id, :inserted_at, and :updated_at into the test’s schema definition. Update your @expected_fields_with_types to include the new fields:

### testing_ecto/test/schemas/user_database_schema_test.exs

```
@expected_fields_with_types [
{:id, :binary_id},
{:date_of_birth, :date},
{:email, :string},
{:favorite_number, :float},
{:first_name, :string},
{:inserted_at, :utc_datetime_usec},
{:last_name, :string},
{:phone_number, :string},
{:updated_at, :utc_datetime_usec}
]
```

After that, the individual test should pass, but what if we run the whole file? Running mix test test/schemas/user_database_schema_test.exs will now yield errors that we haven’t seen yet. Those errors will be for the success test and the test on casting errors, and the output for both will look similar to the following:

```
1: 1) test changeset/1 success: returns a valid changeset when given valid
    arguments (TestingEcto.Schemas.UserDatabaseSchemaTest)
- test/schemas/user_database_schema_test.exs:39
- ** (BadFunctionError) expected a function, got: nil
5: code: valid_params = valid_params(@expected_fields_with_types)
- stacktrace:
- (testing_ecto 0.1.0) test/schema_case.ex:34:
    anonymous fn/3 in TestingEcto.SchemaCase.valid_params/1
- (elixir 1.10.2) lib/enum.ex:2111:
    Enum."-reduce/3-lists^foldl/2-0-"/3
10:
- (testing_ecto 0.1.0) test/schema_case.ex:33:
    TestingEcto.SchemaCase.valid_params/1
- test/schemas/user_database_schema_test.exs:40: (test)
```

The key to understanding these errors is line 4, where we see that the code was expecting a function but got nil instead. Combined with the top of the stacktrace (line 7), we can get a sense of what is breaking and where in the code it’s happening. Our helper function,
