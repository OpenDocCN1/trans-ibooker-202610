# 第 5 章

## 测试 Ecto 查询

测试 Ecto 查询相当直接，特别是如果你来自其他语言的数据库代码测试。一些基本规则将帮助你确保你的覆盖率有效。在本章中，我们将探索一些额外的工具，这些工具可以帮助使测试套件的长期维护更轻松，然后我们将为 Ecto 查询编写测试。我们将基于上一章、第 4 章《测试 Ecto Schemas》中的概念。虽然严格来说不需要阅读那一章，但如果没有阅读，你可能会错过一些我们已经涵盖的代码或概念的引用。我们已随书提供了上一章代码的完整示例，因此在需要时，你可以使用它作为参考。

对于本章，我们将使用一个名为 testing_ecto/lib/schemas/user.ex 的模式，该模式在 testing_ecto 的副本中提供。它是我们上一章结束时模式的演进，有两个显著变化：现在我们已经完成了对模式的更改，我们称之为 User，并且它具有 changeset 函数 create_changeset/1 和 update_changeset/2，而不是单个多用途函数。我们还包含了一个更新的测试文件 testing_ecto/test/schemas/user_test.exs。将这两个文件复制到你的本地 testing_ecto 项目中。测试文件假设你也有 testing_ecto/test/schema_case.ex 的副本，因此如果你跳过了上一章的工作，你也需要将其复制过来。



接下来，我们将创建一个工厂文件，使测试的设置阶段更简单、更一致。然后我们将创建一个新的案例模板，并使用工厂和案例模板两者来测试基本的数据库查询代码。

## 创建工厂以帮助设置

在测试中，我们创建工厂，以便为设置阶段提供数据时拥有一致、可重复的方式。这种工具最有用的领域之一是在与数据库交互时。

当我们编写与数据库表交互的测试时，我们需要两组数据，这些数据相对于表的定义而言是有效或无效的。随着应用程序的增长，我们会发现拥有单一的设置数据源有助于保持测试套件的可维护性。如果我们更新模式定义，那么这个单一来源通常是测试套件中唯一需要更新的地方（除了模式文件本身的测试之外）。

完全可以从头开始为你的应用程序编写工厂。当我们测试 Ecto 模式时，我们就是通过创建一个名为 valid_params 的辅助函数来开始做的。它接收一组字段和类型，并返回一组随机参数，这些参数满足模式定义的最少要求。然而，该函数有一些明显的局限性。它被编写为通用，因此它返回的数据并不非常特定于为其生成数据的确切字段。一个例子是，由于 email 字段是一个字符串，该函数返回一个单字字符串，而不是一个电子邮件地址。这不一定会成为问题，但奇怪的数据会使失败测试的调试变得有点困难。此外，当我们有一个测试需要将数据插入数据库时，我们还必须编写执行插入的代码。

我们可以通过创建处理插入的通用代码来解决这个问题。虽然你可以从头开始编写工厂，但我们不建议这样做。如果你走这条路，你会不断发现边缘情况（例如对值的覆盖或处理嵌套模式），这需要你继续更新和升级你的工厂。许多人已经努力解决这些问题，我们可以基于他们的工作进行构建。虽然没有完美的解决方案，但我们已决定使用 Thoughtbot 的 ExMachina 库作为我们工厂的基础。[36]

ExMachina 开箱即带来相当多的功能，尽管它也有一些值得注意的怪癖。我们将通过将其整合到我们的 TestingEcto 应用程序中来介绍 ExMachina 的基本用例，然后我们将展示如何用额外的數據函数补充你的基于 ExMachina 的工厂。ExMachina 最初是为生成 Ecto 模式的数据而设计的。因此，它非常适合为模式本身生成数据，同时还提供一种插入数据的机制，这样你就不必编写自己的代码。

首先，我们将在与上一章相同的项目中工作。我们需要将 ExMachina 添加为应用程序依赖项。打开你的 mix 文件 (testing_ecto/mix.exs) 并在 deps/0 私有函数中的列表中插入 {:ex_machina, "~> 2.4", only: :test},。运行 mix deps.get 将库代码拉入你的项目。这就足够让我们开始使用该库了。

现在让我们添加自己的工厂文件。在 testing_ecto/test/factory.ex 创建一个新的工厂文件。插入以下代码，然后我们将介绍其中的重要部分：

```
testing_ecto/test/factory.ex

defmodule TestingEcto.Factory do
  use ExMachina.Ecto, repo: TestingEcto.Repo
  alias TestingEcto.Schemas.User

  def user_factory do
    %User{
      date_of_birth: to_string(Faker.Date.date_of_birth()),
      email: Faker.Internet.email(),
      favorite_number: :rand.uniform() * 10,
      first_name: Faker.Name.first_name(),

      last_name: Faker.Name.last_name(),
      phone_number: Faker.Phone.EnUs.phone(),
    }
  end
end
```

除非你需要不止一个工厂文件（随着应用程序的增长，这对于关注点分离很有用），否则坚持使用 Factory 这个名称应该就足够了。在第 2 行，我们的模块“使用” ExMachina.Ecto 并传递我们应用程序的 repo 的名称 TestingEcto.Repo。这意味着这个工厂将专门与该 repo 一起使用。在连接到多个数据库的应用程序中，你需要有多个工厂，并为每个工厂命名得更具体一些。

我们添加了一个单一的工厂函数（第 5 行），遵循 ExMachina 的约定：模式名称后跟“_factory”。ExMachina 在内部使用元编程；为了使其工作，这一约定是必须的。这最终会在你实际使用工厂函数时造成轻微的脱节，我们稍后会指出这一点。现在只需知道，工厂文件中需要与 ExMachina 配合工作的任何函数都必须附加“_factory”。

该函数本身非常简单；它只是返回一个预填充值的模式结构体。如果我们按名称调用此函数，它将返回一个结构体。这本身很有用，但这不是我们需要另一个库的原因。只要你遵循命名约定，ExMachina 就提供了许多辅助函数，我们将在本章的其余部分中使用它们。它提供的不同功能分为两个阶段。首先，它根据工厂默认值和传入的任何覆盖生成数据集。当数据准备好后，它会执行函数调用的操作，例如插入记录或创建字符串键映射。虽然初始数据集基于相同的结构体，但返回值和副作用由你调用的函数决定。随着我们在本章的进展，我们将使用不同的 ExMachina 函数，并在使用时进行解释。

在你开发测试套件时，工厂可能是添加额外辅助函数的好地方，例如上一章中的 invalid_data/1。任何专注于数据并且可在测试之间重用的东西都是移入工厂的好候选。工厂可以直接引入，也可以通过包含它的案例模板来使用。

## 添加 DataCase 以帮助设置

在测试我们的 Ecto 模式时，我们创建了一个名为 SchemaCase 的案例模板。那非常特定于一种确切的测试类型。虽然创建它的练习很有用，但你可能会发现一个稍微更高级别的案例模板可以满足你在数据库交互所需的数据处理方面的需求。我们将创建一个新的案例模板，它看起来会非常相似，如果你花时间的话，就能够替换我们在模式测试中使用的 SchemaCase。创建一个名为 testing_ecto/test/data_case.ex 的新文件并添加以下代码：

```
testing_ecto/test/data_case.ex

defmodule TestingEcto.DataCase do
  use ExUnit.CaseTemplate

  using do
    quote do
      alias Ecto.Changeset
      import TestingEcto.DataCase
      alias TestingEcto.{Factory, Repo}
    end
  end

  setup_tags? do
    Ecto.Adapters.SQL.Sandbox.mode(TestingEcto.Repo, :manual)
  end
end
```

现在，使用此文件相比使用 ExUnit.Case 只有两个优势。首先，它提供了通用的沙箱设置，就像我们在上一章中 SchemaCase 所做的那样。其次，它在第 8 行为 TestingEcto.Factory 和 TestingEcto.Repo 添加了别名。我们在查询测试中都需要这两个。我们在构建测试套件时，请继续寻找可以移入公共文件以实现重用的辅助函数。一个好的经验法则是，如果一个函数专注于生成数据，它可以进入工厂。如果不是，它可能属于案例模板。如果你发现同一个函数需要在多个案例模板中使用，你可能有一个可以提取到多个案例模板中的辅助函数模块的候选。

现在我们有了工厂和案例模板，我们可以开始着手测试我们的查询代码了。



## 测试创建

我们将从查看包含我们查询的逻辑文件的一部分开始。该文件名为 Users，包含基本的 CRUD 操作，在本例中称为 create/1、get/1、update/2 和 delete/1。我们在书中附带的 testing_ecto 副本中提供了一个文件。您可以复制该文件，也可自行输入。该文件和 create/1 函数如下所示：

```
testing_ecto/lib/users/users.ex

defmodule TestingEcto.Users do
  @moduledoc false
  alias TestingEcto.Repo
  alias TestingEcto.Schemas.User

  def create(params) do
    params
    |> User.create_changeset()
    |> Repo.insert()
  end
end
```

它非常基础，就像大多数 CRUD 查询一样。因此，测试它也不会很复杂。首先我们将编写一个成功路径测试。在 testing_ecto/test/users/users_test.exs 处新建一个文件。在其中，我们将设置一个基本的测试文件结构，然后为 create/1 添加一个 describe 块和我们的第一个测试：

```
testing_ecto/test/users/users_test.exs

1: defmodule TestingEcto.UsersTest do
-   use TestingEcto.DataCase
-   alias TestingEcto.Users
-   alias TestingEcto.Schemas.User
5:
-   setup do
-     Ecto.Adapters.SQL.Sandbox.checkout(TestingEcto.Repo)
-   end
```

```
describe "create/1" do
  test "success: it inserts a user in the db and returns the user" do
    params = Factory.string_params_for(:user)
    now = DateTime.utc_now()

    assert {:ok, %User{}} = returned_user = Users.create(params)

    user_from_db = Repo.get(User, returned_user.id)
    assert returned_user == user_from_db

    mutated = ["date_of_birth"]

    for {param_field, expected} <- params,
        param_field not in mutated do
      schema_field = String.to_existing_atom(param_field)
      actual = Map.get(user_from_db, schema_field)

      assert actual == expected,
        "Values did not match for field: #{param_field}\nexpected: #{inspect(expected)}\nactual: #{inspect(actual)}"
    end

    expected_dob = Date.from_iso8601!(params["date_of_birth"])
    assert user_from_db.date_of_birth == expected_dob

    assert user_from_db.inserted_at == user_from_db.updated_at
    assert DateTime.compare(now, user_from_db.inserted_at) == :lt
  end
end
```

```
40:    end
  -
  - end
```

在此测试文件中首先需要注意的是第 7 行的通用 setup 块。因为此测试文件用于我们的查询，我们可以安全地假设每个测试都需要数据库连接。由于我们的 case 模板 DataCase 将 Ecto Sandbox 设置为 :manual，我们需要为每个测试签出一个连接。请记住，在测试结束时，连接将被释放，并且保存我们所有交互的数据库事务将被回滚。

我们在测试本身中看到了第一次使用工厂函数，在第 12 行。我们正在调用 string_params_for/2（即使我们只传递了一个参数，仍有一个可选的第二个参数）。在内部，它接收传入的原子，在本例中为 :user，并将在我们的工厂中调用 user_factory/0 在我们的工厂中。尽管我们的函数 user_factory/0 返回一个 User schema，ExMachina 会将返回值转换为字符串键映射以提供“string_params”。还有一个类似的函数叫 params_for/2，它返回的是原子键映射。我们请求字符串参数，因为这就是大多数基于 Web 的输入的形状。

在此测试中开始出现一种通用模式。当测试正在执行向数据库写入的代码时，测试本身需要在断言阶段从数据库读取以验证副作用。如果测试正在执行从数据库读取的代码，则测试需要在设置阶段直接向数据库写入。不要使用您自己的应用程序代码来进行这些读写操作。在第 17 行，您会看到我们的测试直接调用 TestingEcto.Repo。您不希望测试依赖于应用程序代码。如果您确实使用了应用程序代码并且代码中的某些内容发生变化，导致其功能中断，您的测试套件将出现大量失败的测试。其中大多数会具有误导性，因为它们不会专注于失败代码的测试。我们在设计测试时的目标之一是，如果出现失败，可以快速轻松地找到代码行为不正确的[content]
## 测试创建

我们将从查看包含我们查询的逻辑文件的一部分开始。该文件名为 Users，包含基本的 CRUD 操作，在本例中称为 create/1、get/1、update/2 和 delete/1。我们在书中附带的 testing_ecto 副本中提供了一个文件。您可以复制该文件，也可自行输入。该文件和 create/1 函数如下所示：

```
testing_ecto/lib/users/users.ex

defmodule TestingEcto.Users do
  @moduledoc false
  alias TestingEcto.Repo
  alias TestingEcto.Schemas.User

  def create(params) do
    params
    |> User.create_changeset()
    |> Repo.insert()
  end
end
```

它非常基础，就像大多数 CRUD 查询一样。因此，测试它也不会很复杂。首先我们将编写一个成功路径测试。在 testing_ecto/test/users/users_test.exs 处新建一个文件。在其中，我们将设置一个基本的测试文件结构，然后为 create/1 添加一个 describe 块和我们的第一个测试：

```
testing_ecto/test/users/users_test.exs

1: defmodule TestingEcto.UsersTest do
-   use TestingEcto.DataCase
-   alias TestingEcto.Users
-   alias TestingEcto.Schemas.User
5:
-   setup do
-     Ecto.Adapters.SQL.Sandbox.checkout(TestingEcto.Repo)
-   end
```

```
describe "create/1" do
  test "success: it inserts a user in the db and returns the user" do
    params = Factory.string_params_for(:user)
    now = DateTime.utc_now()

    assert {:ok, %User{}} = returned_user = Users.create(params)

    user_from_db = Repo.get(User, returned_user.id)
    assert returned_user == user_from_db

    mutated = ["date_of_birth"]

    for {param_field, expected} <- params,
        param_field not in mutated do
      schema_field = String.to_existing_atom(param_field)
      actual = Map.get(user_from_db, schema_field)

      assert actual == expected,
        "Values did not match for field: #{param_field}\nexpected: #{inspect(expected)}\nactual: #{inspect(actual)}"
    end

    expected_dob = Date.from_iso8601!(params["date_of_birth"])
    assert user_from_db.date_of_birth == expected_dob

    assert user_from_db.inserted_at == user_from_db.updated_at
    assert DateTime.compare(now, user_from_db.inserted_at) == :lt
  end
end
```

```
40:    end
  -
  - end
```

在此测试文件中首先需要注意的是第 7 行的通用 setup 块。因为此测试文件用于我们的查询，我们可以安全地假设每个测试都需要数据库连接。由于我们的 case 模板 DataCase 将 Ecto Sandbox 设置为 :manual，我们需要为每个测试签出一个连接。请记住，在测试结束时，连接将被释放，并且保存我们所有交互的数据库事务将被回滚。

我们在测试本身中看到了第一次使用工厂函数，在第 12 行。我们正在调用 string_params_for/2（即使我们只传递了一个参数，仍有一个可选的第二个参数）。在内部，它接收传入的原子，在本例中为 :user，并将在我们的工厂中调用 user_factory/0。尽管我们的函数 user_factory/0 返回一个 User schema，ExMachina 会将返回值转换为字符串键映射以提供“string_params”。还有一个类似的函数叫 params_for/2，它返回的是原子键映射。我们请求字符串参数，因为这就是大多数基于 Web 的输入的形状。

在此测试中开始出现一种通用模式。当测试正在执行向数据库写入的代码时，测试本身需要在断言阶段从数据库读取以验证副作用。如果测试正在执行从数据库读取的代码，则测试需要在设置阶段直接向数据库写入。不要使用您自己的应用程序代码来进行这些读写操作。在第 17 行，您会看到我们的测试直接调用 TestingEcto.Repo。您不希望测试依赖于应用程序代码。如果您确实使用了应用程序代码并且代码中的某些内容发生变化，导致其功能中断，您的测试套件将出现大量失败的测试。其中大多数会具有误导性，因为它们不会专注于失败代码的测试。我们在设计测试时的目标之一是，如果出现失败，可以快速轻松地找到代码行为不正确的[content]
## 测试创建

我们将从查看包含我们查询的逻辑文件的一部分开始。该文件名为 Users，包含基本的 CRUD 操作，在本例中称为 create/1、get/1、update/2 和 delete/1。我们在书中附带的 testing_ecto 副本中提供了一个文件。您可以复制该文件，也可自行输入。该文件和 create/1 函数如下所示：

```
testing_ecto/lib/users/users.ex

defmodule TestingEcto.Users do
  @moduledoc false
  alias TestingEcto.Repo
  alias TestingEcto.Schemas.User

  def create(params) do
    params
    |> User.create_changeset()
    |> Repo.insert()
  end
end
```

它非常基础，就像大多数 CRUD 查询一样。因此，测试它也不会很复杂。首先我们将编写一个成功路径测试。在 testing_ecto/test/users/users_test.exs 处新建一个文件。在其中，我们将设置一个基本的测试文件结构，然后为 create/1 添加一个 describe 块和我们的第一个测试：

```
testing_ecto/test/users/users_test.exs

1: defmodule TestingEcto.UsersTest do
-   use TestingEcto.DataCase
-   alias TestingEcto.Users
-   alias TestingEcto.Schemas.User
5:
-   setup do
-     Ecto.Adapters.SQL.Sandbox.checkout(TestingEcto.Repo)
-   end
```

```
describe "create/1" do
  test "success: it inserts a user in the db and returns the user" do
    params = Factory.string_params_for(:user)
    now = DateTime.utc_now()

    assert {:ok, %User{}} = returned_user = Users.create(params)

    user_from_db = Repo.get(User, returned_user.id)
    assert returned_user == user_from_db

    mutated = ["date_of_birth"]

    for {param_field, expected} <- params,
        param_field not in mutated do
      schema_field = String.to_existing_atom(param_field)
      actual = Map.get(user_from_db, schema_field)

      assert actual == expected,
        "Values did not match for field: #{param_field}\nexpected: #{inspect(expected)}\nactual: #{inspect(actual)}"
    end

    expected_dob = Date.from_iso8601!(params["date_of_birth"])
    assert user_from_db.date_of_birth == expected_dob

    assert user_from_db.inserted_at == user_from_db.updated_at
    assert DateTime.compare(now, user_from_db.inserted_at) == :lt
  end
end
```

```
40:    end
  -
  - end
```

在此测试文件中首先需要注意的是第 7 行的通用 setup 块。因为此测试文件用于我们的查询，我们可以安全地假设每个测试都需要数据库连接。由于我们的 case 模板 DataCase 将 Ecto Sandbox 设置为 :manual，我们需要为每个测试签出一个连接。请记住，在测试结束时，连接将被释放，并且保存我们所有交互的数据库事务将被回滚。

我们在测试本身中看到了第一次使用工厂函数，在第 12 行。我们正在调用 string_params_for/2（即使我们只传递了一个参数，仍有一个可选的第二个参数）。在内部，它接收传入的原子，在本例中为 :user，并将在我们的工厂中调用 user_factory/0。尽管我们的函数 user_factory/0 返回一个 User schema，ExMachina 会将返回值转换为字符串键映射以提供“string_params”。还有一个类似的函数叫 params_for/2，它返回的是原子键映射。我们请求字符串参数，因为这就是大多数基于 Web 的输入的形状。

在此测试中开始出现一种通用模式。当测试正在执行向数据库写入的代码时，测试本身需要在断言阶段从数据库读取以验证副作用。如果测试正在执行从数据库读取的代码，则测试需要在设置阶段直接向数据库写入。不要使用您自己的应用程序代码来进行这些读写操作。在第 17 行，您会看到我们的测试直接调用 TestingEcto.Repo。您不希望测试依赖于应用程序代码。如果您确实使用了应用程序代码并且代码中的某些内容发生变化，导致其功能中断，您的测试套件将出现大量失败的测试。其中大多数会具有误导性，因为它们不会专注于失败代码的测试。我们在设计测试时的目标之一是，如果出现失败，可以快速轻松地找到代码行为不正确的

```
assert {:error, %Changeset{valid?: false}} =
    Users.create(missing_params)
end
```

这个测试可以如此简单，因为我们已经对 schema 的错误情况进行了充分测试。Ecto 标准化了 Repo 错误和 Changeset 错误的处理方式。我们所做的测试只是确保如果出现错误，返回值具有正确的形状：一个以 :error 作为第一个元素的元组，以及一个携带错误的 changeset 作为第二个元素。如果其他任何函数使用 schema 上相同的底层 changeset 函数，它们就可以做出相同的假设，除非函数本身需要改变行为。

这两种测试建立了一个模式，该模式将延续到本文件中其余函数的测试中：一个成功测试和一个错误测试。让我们测试下一个函数，看看它是如何运行的。

## 测试读取

下一个测试非常简单。首先，让我们看看 Users 模块中的代码。我们在节标题中使用了“Read”以保持与 CRUD 一致，但你会注意到我们更喜欢将函数命名为 get/1。这完全是用户偏好。

```
testing_ecto/lib/users/users.ex

def get(user_id) do
  if user = Repo.get(User, user_id) do
    {:ok, user}
  else
    {:error, :not_found}
  end
end
```

我们的函数并不是对 Ecto 代码的直接传递。相反，如果查询返回结果，它会将该结果包装在成功 (:ok) 元组中，如果用户不存在，则返回错误元组。我们的测试也将非常直接。对于成功情况，我们需要在执行步骤之前将用户插入数据库。我们将断言返回值的形状，然后确保实际数据与之前插入数据库的数据匹配。错误测试将尝试为不存在的 ID “获取”用户。打开你的测试文件，testing_ecto/test/users/users_test.ex，并添加以下 describe 块：

```
1:  describe "get/1" do
    -  test "success: it returns a user when given a valid UUID" do
    -    existing_user = Factory.insert(:user)
    - 
5:    assert {:ok, returned_user} = Users.get(existing_user.id)
    - 
    -  assert returned_user == existing_user
    -  end
    - 
    test "error: it returns an error tuple when a user doesn’t exist" do
10:
+   - assert {:error, :not_found} = Users.get(Ecto.UUID.generate())
+   - end
+   - end
```

这些测试很容易理解。因为被测试的函数是从数据库读取，所以我们的测试代码需要在执行步骤之前插入数据。我们在第 3 行使用工厂将数据插入数据库。使用工厂的好处之一是，它提供了一种简单且一致的方式将数据插入数据库，而无需依赖你自己的应用程序代码。

不过需要注意的是，因为它没有使用你的应用程序代码，所以插入的数据不会经过你的 changeset 函数。如果数据规则存在于 changeset 逻辑中但未反映在数据库中，你需要确保工厂函数只返回有效数据。这就是测试读取函数。

## 测试更新

测试更新有点结合了对 create 和 read 函数的测试。让我们先看看我们要测试的函数：

```
testing_ecto/lib/users/users.ex

def update(%User{} = existing_user, update_params) do
  existing_user
  |> User.update_changeset(update_params)
  |> Repo.update()
end
```

你可以看到，与 create/1 类似，这个函数是对 schema 的 changeset 函数（本例中为 update_changeset/2）的调用以及对 Repo.update/1 的调用的轻量级包装。测试方法类似，但你需要插入一个现有用户以便代码对其进行更新。我们先编写成功测试：

```
describe "update/2" do
  test "success: it updates database and returns the user" do
    existing_user = Factory.insert(:user)

    params =
      Factory.string_params_for(:user)
      |> Map.take(["first_name"])

    assert {:ok, returned_user} = Users.update(existing_user, params)

    user_from_db = Repo.get(User, returned_user.id)
    assert returned_user == user_from_db

    expected_user_data =
      existing_user
      |> Map.from_struct()
      |> Map.drop([:__meta__, :updated_at])
      |> Map.put(:first_name, params["first_name"])

    for {field, expected} <- expected_user_data do
      actual = Map.get(user_from_db, field)

      assert actual == expected,
        "Values did not match for field: #{field}\n expected: #{inspect(expected)}\nactual: #{inspect(actual)}"
    end

    refute user_from_db.updated_at == existing_user.updated_at
    assert %DateTime{} = user_from_db.updated_at
  end
end
```

测试在第 3 行使用工厂插入一个用户。在第 5 行，测试正在创建要传入的参数映射。它使用工厂提供数据以保持一致性，但随后使用 Map.take/2 只获取一个键/值对。这是为了让测试尽可能简单且不易过时。如果测试更新了 schema 允许的每个字段，那么随着 schema 的任何更改，测试过时的可能性就会更高。通过选择一个字段，并且是一个不太可能更改的字段，此测试将更容易保持最新。此外，对 update_changeset/2 的测试确实涵盖了哪些字段可以更新的逻辑，因此我们不需要在此测试中过于健壮。

我们的测试需要断言所有原始字段的值都相同，除了 :first_name（已更新的字段）和 :updated_at（将单独处理）。为此，最简单的方法是构造一个包含预期键和值的映射，然后将其与从数据库中提取的值进行比较。有很多方法可以做到这一点，但我们选择从现有用户开始，因为它包含了我们想要的大部分数据。

在第 14 行，我们将该用户的数据转换为映射，然后更新发生变化的那个值。在第 17 行，测试删除了两个字段，意味着它们不会被检查。:updated_at 被删除，因为它在两组数据之间不会相同，而且也不需要相同。:__meta__ 被删除，即使它在两组数据上是相同的，因为它是一个隐藏的 Ecto Schema 字段，不属于测试的关注范围。测试关注的是存在于数据库中的数据。

在第 29 行，我们进行了一个非常基本的断言，以确保 :updated_at 字段的值已更改，并且数据仍然具有正确的形状，即 DateTime 结构体。这与我们在测试 create/1 时讨论的测试时间戳相关。我们在保持高覆盖率和减少断言的工作量和维护量之间取得平衡。

让我们接下来添加错误测试。在同一个 update/2 describe 块中，在成功测试下方添加一个如下所示的测试：

```
```elixir
1: test "error: returns an error tuple when user can’t be updated" do
2:   existing_user = Factory.insert(:user)
3:   bad_params = %{"first_name" => DateTime.utc_now()}
4:
5:   assert {:error, %Changeset{}} = Users.update(existing_user, bad_params)
6:
7:   assert existing_user == Repo.get(User, existing_user.id)
8: end
```
```

这个测试很容易理解。在第 3 行，它创建了一个无法转换的参数。这与我们在 SchemaCase 文件中通过 schema 测试调用的辅助函数 invalid_params/1 中的逻辑相同。此测试没有使用相同的 case 模板。如果你经常在查询测试中做这类事情，它可以成为 DataCase 或工厂中的一个函数的良好候选，因为它关注的是数据。

在测试失败的更新时，一个常见的错误（就像这个测试所做的）是忘记检查数据库中的数据是否未发生变化。鉴于 Users.update/2 中的逻辑很少，几乎不可能有任何变化，但这对我们来说几乎不需要付出努力，并且可以防止回归。第 7 行显示了一个单行断言和对数据库的调用，以增加这种安全性。

我们已经涵盖了测试更新函数的要点。让我们最后测试一下删除用户的逻辑。



## 测试删除

我们最后一个函数 delete/1 很简单就能测试，因为代码直接传递给 Repo.delete/1。让我们来看一下该函数：

```
testing_ecto/lib/users/users.ex

def delete(%User{} = user) do
  Repo.delete(user)
end
```

我们将只为此函数测试成功路径，因为错误极不可能发生。此外，编写一个可以强制在删除时产生错误的测试很复杂，并且需要仅为测试而重构我们的代码。在这种情况下，收益不大。当出现这种情况时，我们通常让应用程序崩溃，而不是试图处理每一种可能的失败。让我们编写最终的测试，然后回顾它做了什么：

```
describe "delete/1" do
  test "success: it deletes the user" do
    user = Factory.insert(:user)

    assert {:ok, _deleted_user} = Users.delete(user)

    refute Repo.get(User, user.id)
  end
end
```

我们的测试通过插入一个用户来设置。然后它执行 delete/1 函数，并断言它返回一个成功元组。最后一行是断言数据已不再存在于数据库中。在我们的例子中，我们甚至不关心返回值，所以我们将其丢弃。最重要的是要确保你对数据库进行了最后一次检查。

## 总结

在本章中，我们介绍了基本的 CRUD 函数以及如何测试它们。我们查看了使用通用设置源，即工厂。我们看到了如何对返回值和数据库中的数据做出彻底的断言。虽然没有两个应用程序会具有完全相同的功能，但这些示例概述了你可以构建的基础，以确保你的查询得到测试覆盖。

在第 4 章 *测试 Ecto 模式* 和本章之间，我们讨论了在测试你的 Ecto 代码时应牢记的主要问题。随着你构建更大更复杂的查询代码，你只需要记住你的测试需要同时覆盖返回值和副作用。未能检查副作用（数据库中的更改）是我们看到的最常见错误之一，也是测试查询代码中最重要的一部分。

在下一章中，我们将转向测试 Phoenix 应用程序，在这里和前几章中我们已经概述了所有概念的基础上进行构建。

## 脚注

[36] https://hex.pm/packages/ex_machina
