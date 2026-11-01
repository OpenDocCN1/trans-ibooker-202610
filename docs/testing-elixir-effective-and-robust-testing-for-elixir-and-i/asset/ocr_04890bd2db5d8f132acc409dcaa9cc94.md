perfect solution, we have settled on using the ExMachina library from Thoughtbot as the basis for our factories.[36]

ExMachina brings a good bit of functionality out of the box, although it also has some notable quirks. We’ll run through the basic use cases of ExMachina by incorporating it into our TestingEcto application, and then we’ll show how you supplement your ExMachina-based factory with additional data functions. ExMachina was originally designed with producing data for Ecto schemas in mind. As a result, it works well for producing data for the schemas themselves, as well as providing a mechanism to insert data so that you don’t have to write your own code.

To begin, we’ll be working in the same project as the previous chapter. We need to add ExMachina as an application dependency. Open your mix file (testing_ecto/mix.exs) and insert {:ex_machina, "~> 2.4", only: :test}, into the list in the deps/0 private function. Run mix deps.get to pull the library code into your project. That’s enough for us to start using the library.

Now let’s add our own factory file. Create a new file for your factory at testing_ecto/test/factory.ex. Insert the following code and then we’ll run through the notable parts:

```
testing_ecto/test/factory.ex

1: defmodule TestingEcto.Factory do
  -   use ExMachina.Ecto, repo: TestingEcto.Repo
  -   alias TestingEcto.Schemas.User
  -
5: def user_factory do
  -   %User{
  -     date_of_birth: to_string(Faker.Date.date_of_birth()),
  -     email: Faker.Internet.email(),
  -     favorite_number: :rand.uniform() * 10,
  -     first_name: Faker.Name.first_name(),
10:
  -     last_name: Faker.Name.last_name(),
```