
```
assert meta[:validation] == :cast,
  "The validation type, #{meta[:validation]}, is incorrect."
end
end
```

唯一实际的更改在执行代码的行（4）上，但该行上有几处不同的内容。首先，我们需要更新 `=` 左侧的模式，将 changeset 用以 `:error` 作为第一个元素的元组包裹起来。我们的测试不再关心是否返回一个无效的 changeset；它只关心如果得到一个错误元组，则第二个元素是一个带有错误的 changeset。因此，我们可以从模式中移除 `valid?: false`。最后，将调用的函数从 `changeset/1` 更新为 `cast_and_validate/1`。现在测试应该可以正常运行并通过。

最后一个测试应做同样的修改，然后看起来像这样：

```
test "error: returns error changeset when required fields are missing" do
  params = %{}

  assert {:error, %Changeset{errors: errors}} =
    UserValidator.cast_and_validate(params)

  for {field, _} <- @expected_fields_with_types, field not in @optional do
    assert errors[field], "The field :#{field} is missing from errors."
    {_, meta} = errors[field]

    assert meta[:validation] == :required,
      "The validation type, #{meta[:validation]}, is incorrect."
  end
end
```

所有测试现在都应该通过了。你可以看到，我们的测试主要概念与测试基本模式时保持一致。测试只是需要更新以反映 UserValidator 接口的微小变化。

接下来我们将回到基本模式并修改它以用于数据库交互。然后我们将讨论测试数据库模式的方法。

## 为数据库交互测试 Ecto 模式

Ecto 模式最常见的用例是实现与数据库的轻松交互。虽然我们将模式的逻辑保持在最小限度——仅模式定义和 changeset 函数或函数——但为了使用模式与数据库，我们仍需要做一些更改。我们将更新我们编写的基本模式（跳过我们为将其用作验证器所做的更改），然后更新我们的测试。一旦我们涵盖了模式方面，我们将继续介绍使用模式的查询测试。

## 在我们的应用程序中添加数据库

在我们进行必要的更改之前，我们需要确保有一个本地运行的数据库。鉴于每个读者本地设置都不同，我们将利用 Docker 来降低因本地环境差异导致问题的可能性。我们必须假设你已经在电脑上安装了 Docker 或能够设置它。[31] 我们已随 testing_ecto 应用程序代码提供了两个文件，你应将其复制到应用程序目录：testing_ecto/docker-compose.yml 和 testing_ecto/Makefile。将其保留在本地 testing_ecto 应用程序的根级别。

docker-compose.yml 文件具有启动和运行 Postgres 容器的规范，该容器与本书中使用的用户名和密码相匹配。为了方便，我们包含了一个 Makefile，为你提供简单的命令以快速启动。文件就位后，在同一目录中打开一个新的 shell 会话。这将使我们在数据库和应用程序之间切换变得更容易。运行 make up 以下载镜像，然后创建并启动一个运行中的 Postgres 容器。你应该会看到镜像被下载（第一次运行时）然后确认它正在运行。Make 命令以守护进程模式运行它，这意味着它在后台运行，直到你明确关闭它。稍后，当你准备好关闭时，运行 make down 即可处理。唯一可能遇到麻烦的地方是如果你在电脑上运行了另一个 Postgres 副本。我们的容器将绑定到 5432 端口，即 Postgres 的默认值。如果出现端口冲突错误，你需要在学习本书示例时关闭其他正在运行的 Postgres。

## 更新我们的应用程序以使用数据库

现在我们有了数据库，我们需要设置应用程序以连接到它。首先，我们需要在 mix.exs 文件的 deps 部分添加两个新依赖项，{ :ecto_sql, "~> 3.4"} 和 {:postgrex, "~> 0.15.0"}。Ecto SQL 为你的应用程序提供连接数据库所需的额外代码，而 Postgrex 提供了 Ecto SQL 需要的适配器，使其能够专门与 Postgres 数据库一起工作。在 testing_ecto 应用程序的根目录运行 mix deps.get 以拉取新的库。

在 testing_ecto 代码库中，你会发现我们包含了一些非常基本的配置文件（testing_ecto/config/config.exs 和 testing_ecto/config/test.exs）以及一个非常基本的 repo 文件（testing_ecto/lib/repo.ex）。这些文件硬编码了将与我们提供的 docker-compose 文件创建的 Postgres 实例配合使用的值。唯一可能不是标准样板的是，我们在数据库配置中添加了选项 migration_timestamps: [type: :utc_datetime_usec]。这将使我们的时间戳默认为 UTC 时区，除非明确覆盖，并在其定义中包含微秒。我们稍后需要记住这一点。将它们复制到你的项目中。

我们需要确保 TestingEcto.Repo 应用程序随我们的应用程序一起启动，以便在后面的测试中与数据库交互。打开 lib/testing_ecto/application.ex，我们将把我们的 repo 添加到 children 中：

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

由于这不是一个真正的应用程序，而只是专注于测试数据库，我们的子进程列表将只包含一个元素，TestingEcto.Repo。现在，当我们运行测试时，我们将有一个正在运行的 repo 应用程序来允许我们与数据库交互。

数据库和 repo 运行后，加上新的库和配置文件（config/config.exs 和 config/test.exs），你的应用程序应该准备好连接和与数据库交互。接下来，从任一 shell 会话运行 MIX_ENV=test mix ecto.create 以创建名为 testing_ecto_test 的测试数据库实例。你应该会看到以下输出：

```
Generated testing_ecto app

# The database for TestingEcto.Repo has been created
```

每当你的应用程序对数据库结构进行更改时，检查预期更改是否反映在数据库中是良好的实践。在第二个 shell 会话中，运行 `make cli` 以连接到 Docker 容器内的 Postgres cli 客户端。如果能够使用此命令连接，则可验证数据库是否已正确创建。由于我们只创建了数据库，现在没有太多事情可做，除了运行 `\l` 列出可用数据库。`testing_ecto_test` 应该在列表中。现在我们需要在数据库中添加一个表，以与我们正在设置的模式相对应。回到应用程序目录的命令行，运行 `mix ecto.gen.migration add_users_table`。这将生成嵌套目录和一个带时间戳名称的文件，如以下输出所示（你会有不同的时间戳）：

```
Generated testing_ecto app
* creating priv/repo/migrations
* creating priv/repo/migrations/20200518212741_add_users_table.exs
```

打开文件，你应该会看到一个迁移文件，其中包含一个空的 `change/0` 函数。让我们添加代码来创建一个与我们的模式匹配的表。在 `change/0` 函数内部，添加代码使你的文件与以下内容匹配：

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
