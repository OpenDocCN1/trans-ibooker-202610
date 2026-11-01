tests. Open up lib/testing_ecto/application.ex and we’ll add our repo into the children:

```
testing_ecto/lib/testing_ecto/application.ex

1: defmodule TestingEcto.Application do
  - # See https://hexdocs.pm/elixir/Application.html
  - # for more information on OTP Applications
  - @moduledoc false
5:
  - use Application
  - 
  - def start(_type, _args) do
  -   children = [
      TestingEcto.Repo
10:
  -   ]
  - 
  -   opts = [strategy: :one_for_one, name: TestingEcto.Supervisor]
  -   Supervisor.start_link(children, opts)
  end
15:
  - end
```

Since this isn’t a real application but is just focused on testing the database, our list of children will just contain the one element, TestingEcto.Repo. Now, when we run our tests, we’ll have a running repo application to allow us to interact with the database.

With database and repo running, plus the new libraries and the configuration files (config/config.exs and config/test.exs), your application should be ready to connect and interact with the database. Next, run MIX_ENV=test mix ecto.create from either shell session to create the test instance of the database, called testing_ecto_test. You should see the following output:

Generated testing_ecto app