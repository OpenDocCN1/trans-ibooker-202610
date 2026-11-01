# Chapter 4

## Testing Ecto Schemas

随着 Elixir 的成熟，社区正在为某些用例确定某些库作为 *该* 库。既因为 José Valim 的参与，也因为它被包含在 Phoenix 中——事实上的 Elixir Web 框架——Ecto 已被定位为 Elixir 的 *该* 关系数据库库。由于 Ecto 由几个离散部分组成，Ecto.Schema 找到了第二个目的，即作为与数据库相关需求分离的类型转换和验证库。这给我们留下了 Ecto 的两种最常见用例：验证和数据库交互。两种用途都有各自的测试集，我们将在本章中涵盖两者。

常见的说法“不要测试你的框架”常常导致人们对数据库代码测试不足。在开始测试我们的 Ecto 代码之前，让我们审视这一说法并确定其带来的实际影响。Ecto 是 Elixir 社区中使用最广泛的库之一，后面有一支优秀的团队在做非常扎实的工作。该库“就是能用”。如果有 bug，它们会被很快发现。当我们编写测试时，我们 *不应该* 测试库的实际功能，但我们 *应该* 测试我们的代码是否正确使用了该库的功能。

这可能会让人困惑，但我们可以提供一个启发式方法来帮助你判断是否测试代码：它在你的代码库中吗？如果是，

然后通常你应该对其进行测试。如果不是，那么通常就不应该。那意味着你不应该测试 Ecto 中的某个函数是否工作正确，而应该测试该函数是否从你的代码库正确地调用到库代码中。如果你思考使用 Ecto 的代码，你的测试实际上应该专注于该代码所提供的接口。这意味着你的测试在理论上不应该知道或关心你是否在使用该库。它们只需要专注于输入、返回值以及副作用（在本例中，对数据库中数据的更改）。

在我们测试 Ecto 的旅程中，我们将首先将其作为验证库和数据库模式进行测试。然后我们将转向测试你的数据库交互。最后，我们将研究工具、工厂和测试用例，这些都可以大幅加快编写测试的速度。作为奖励，我们还将介绍一些可以在其他测试中使用的测试技巧。

## Ecto 的第二种用途

![](img/c08a369b9a1934f7c9c3b485727eca8e_170_0.png)

自 Ecto 3.0 起，Ecto 团队认可了 Ecto 作为验证库的次要用途。他们实际上将该库拆分为两个部分，允许在不需要时使用模式而无需将数据库特定代码引入你的应用程序。

这种关注点分离是使用离散、可组合的部件构建灵活库的一个很好的例子。

## 通过 Changeset 测试你的模式

正如我们提到的，Ecto 不仅可用于数据库交互，最常见的情况是作为数据的验证器。无论你如何使用它，模式文件中都有一个共同的核心代码，因此存在共同的测试。我们将首先测试该通用代码，然后进行一些重构以使测试更易于维护。之后，我们将分支出并对代码和测试进行两次重构，每种用例一次。我们会在发生时指出。这意味着我们将在非常相似的代码中工作，但会切换文件名以保持概念分离。

我们的基于 Ecto 的代码和测试将体现这样一种信念：模式文件应包含最少的逻辑，通常只是模式定义以及专注于将数据转换为该模式定义或从该模式定义转换数据的函数。如果你喜欢在模块中放置更多逻辑，测试它就由你自己负责了。

Ecto 提供了两个模块，Ecto.Schema 和 Ecto.Changeset，我们将使用它们来定义基本模式和核心验证行为。Ecto.Schema 允许我们使用领域特定语言（DSL）定义（Ecto）结构体及其类型。Ecto.Changeset 是提供类型转换和验证逻辑等代码。当你使用 Ecto.Schema 定义模式时，你是在定义数据的容器，但代码测试专注于行为。在第一次尝试中，我们将测试使用 Ecto.Changeset 函数的代码，以便我们的测试能够探索和验证模式的定义以及单个函数的行为。

让我们从一个基本模式开始，然后看看如何对其进行测试。我们将为用户创建一个通用的 Ecto 模式，并在学习过程中对其进行扩展。创建一个名为 testing_ecto 的新 Mix 项目，并带有一个监督树（$ mix new --sup testing_ecto）。接下来，将 {:ecto, "~> 3.0"} 添加到 mix.exs 文件的 deps 部分。运行 mix deps.get 以确保你已为项目拉取了 Ecto 的本地副本。现在让我们创建一个名为 lib/schemas/user_basic_schema.ex 的文件并添加以下代码：

```
testing_ecto/lib/schemas/user_basic_schema.ex

defmodule TestingEcto.Schemas.UserBasicSchema do
  use Ecto.Schema
  import Ecto.Changeset

  @optional_fields [:favorite_number]

  @primary_key false
  embedded_schema do
    field(:date_of_birth, :date)
    field(:email, :string)
    field(:favorite_number, :float)
    field(:first_name, :string)
    field(:last_name, :string)
    field(:phone_number, :string)
  end

  defp all_fields do
    __MODULE__.__schema__(:fields)
  end

  def changeset(params) do
    %__MODULE__{}
    |> cast(params, all_fields())
    |> validate_required(all_fields() -- @optional_fields)
  end
end
```

如果你之前使用过 Ecto，你可能会注意到我们做了一件有点奇怪的事情：我们使用了 embedded_schema 而不是 schema。这意味着该模式不能用于与数据库交互，因为它没有关联到表。别担心，我们稍后会介绍如何将其与数据库关联，但请记住，现在我们专注于模式的核心部分。

我们还有一个私有函数，使用了 Ecto.Schema 反射函数。[28] 该函数的优点是它始终返回最新的字段列表（作为原子），即使添加了新字段。由于目前我们在 changeset 中转换所有字段，这可能很有帮助。

## 那不是我写模式的方式！

![](img/c08a369b9a1934f7c9c3b485727eca8e_173_0.png)

> 该模式已经做出了可能不符合每个人风格的选择。有些人不喜欢在模式中放置 changeset 函数。其他人会在文件中放置更多逻辑。有些人以不同的方式跟踪必需参数。当用于数据库交互时，通常建议为不同的操作（如插入和更新）拥有不同的 changeset 函数。像 Elixir、Mix 和 Phoenix 一样，Ecto 的编写允许在使用方式上具有灵活性。我们将展示的测试概念无论你如何组织代码都适用；你可能只需要稍微不同地组织测试以反映你的代码。我们建议跟随书中的示例，因为它们将帮助你专注于内容，而不必担心在阅读时进行自定义。

让我们检查一下我们拥有的代码并讨论需要测试的方面。我们的基本模式定义了五个字段：date_of_birth、favorite_number、first_name、last_name 和 phone_number，其中 favorite_number 是唯一的可选字段。除了模式定义之外，我们有一个名为 changeset/1 的单个函数，其中包含两行代码逻辑。我们的测试需要涵盖基本模式字段（及其类型）以及 changeset/1 中的两行逻辑，即转换和验证必需字段的存在。我们可以通过 changeset/1 的接口测试所有这些内容。

让我们继续开始编写一个成功测试。在项目中创建 test/schemas/user_basic_schema_1_test.exs 测试文件。请注意，我们在文件名中添加了 1，因为为了提供包含代码的重构版本，我们必须对测试文件进行版本控制。这打破了测试文件名应与被测代码名称匹配（但以 _test.exs 结尾）的惯例。

如果你计划在原地进行即将到来的重构而不是在新文件中进行，欢迎跳过此测试文件。

```
testing_ecto/test/schemas/user_basic_schema_1_test.exs

1: defmodule TestingEcto.Schemas.UserBasicSchema1Test do
  -   use ExUnit.Case
  -   alias Ecto.Changeset
  -   alias TestingEcto.Schemas.UserBasicSchema
5:
  -   @schema_fields [
  -     :date_of_birth,
  -     :email,
  -     :favorite_number,
  -     :first_name,
10:
  -     :last_name,
  -     :phone_number
  -   ]
  -
  describe "changeset/1" do
15:
  -   test "success: returns a valid changeset when given valid arguments" do
  -     params = %{
  -       "date_of_birth" => "1948-02-28",
  -       "email" => "example@example.com",
  -       "favorite_number" => 3.14,
20:
- "first_name" => "Bob",
- "last_name" => "Matthews",
- "phone_number" => "555-555-5555"
- }
25:
- changeset = UserBasicSchema.changeset(params)
- assert %Changeset{valid?: true, changes: changes} = changeset
- 
- mutated = [:date_of_birth]
30:
- for field <- @schema_fields, field not in mutated do
- actual = Map.get(changes, field)
- expected = params[Atom.to_string(field)]
- assert actual == expected,
"Values did not match for field: #{field}\nexpected: #{
35:
- inspect(expected)
- }\nactual: #{inspect(actual)}"
- end
- 
expected_dob = Date.from_iso8601!(params["date_of_birth"])
40:
- assert changes.date_of_birth == expected_dob
- end
- 
- end
end
45:
```

我们有一个测试文件，其中包含一个模块属性列出了模式上的字段，以及一个 happy path 测试。我们选择在测试中使用本地字段列表而不是以某种方式引用模式，因为它提供了基本的回归保护。如果有人不小心

deletes a line from the schema defining a field, some of our tests will catch it. We’ll see that in a later test, but for now we can file it away.

我们的测试是一个 happy path 测试。我们有一些硬编码的、已知良好的参数。值得注意的是，我们的参数具有字符串键，因为这很常见，因为 Ecto 的类型转换和验证通常发生在外部输入上（如反序列化的 JSON）。我们在第 26 行执行测试，将这些参数传递给 changeset/1 并断言我们得到的是一个有效的 changeset。我们本可以将该行绑定响应到一个变量，并在后续行进行断言，但如果失败时产生的错误足够清晰——尤其是在你有彩色测试输出的情况下——因此我们不需要这样做。

以下测试失败输出显示我们两个 changesets 在 :valid? 值上有所不同。如果你的 shell 中有彩色输出，它会被高亮显示。

```
1) test changeset/1 success: returns a valid changeset when given valid arguments
    (TestingEcto.Schemas.UserBasicSchema1Test)
test/schemas/user_basic_schema_1_test.exs:16
match (=) failed
code: assert %Changeset{valid?: true, changes: changes} = changeset
left: %Ecto.Changeset{changes: changes, valid?: true}
right: %Ecto.Changeset{
    changes: %{date_of_birth: ~D[1948-02-28], first_name: "Bob",
        last_name: "Matthews", phone_number: "555-555-5555"},
    valid?: false,
rest of test output
```

假设响应是一个有效的 changeset，我们希望确保预期的新值存在于返回的 changeset 的 changes 字段中。我们利用列表推导式，使用模块属性 @schema_fields 来提供需要检查的字段。不过我们必须排除出生日期，因为数据本身在被转换为 Ecto 的 :date 类型时会发生变化。因此，我们需要一个额外的断言来验证 changes 中存在正确的日期。

我们的测试是一个 happy path 测试，但它告诉我们的不仅仅是给定正确数据时事情能正常工作。正如我们之前提到的，由于测试只能关注行为（返回值和副作用），我们无法直接测试 schema 的描述。但此测试以及后续的测试只有在 schema 定义正确时才会通过。虽然单个测试无法提供这种保证，但所有针对 changeset/1 的测试在一起时会提供这种覆盖。我们将在处理重构时再来看另一种方法。

我们的 happy path 测试在理论上只有在值被转换时才会通过，因此这是对 changeset/1 函数中 cast/3 调用的成功测试。让我们通过添加针对同一行的错误测试来锁定该行的覆盖率。我们将传递无法正确转换的值，然后查看 changeset 上的错误。将此测试添加到同一个 describe 块中，在你的最后一个测试下方：

```
test "error: returns an error changeset when given un-castable values" do
  not_a_string = DateTime.utc_now()

  params = %{
    "date_of_birth" => "not a date",
    "email" => not_a_string,
    "favorite_number" => "not a number",
    "first_name" => not_a_string,
    "last_name" => not_a_string,
    "phone_number" => not_a_string
  }

  changeset = UserBasicSchema.changeset(params)

  assert %Changeset{valid?: false, errors: errors} = changeset

  for field <- @schema_fields do
    assert errors[field], "expected an error for #{field}"
    {_, meta} = errors[field]
```

```
20:
-   assert meta[:validation] == :cast,
-       "The validation type, #{meta[:validation]}, is incorrect."
-   end
-   end
```

设置与成功测试相似，但你会看到我们找到了不同数据类型，我们知道 Ecto 无法将它们转换为 schema 中定义的类型。我们要执行的行是相同的，后续的模式匹配和断言仅在期望 valid? 为 false 方面有所不同。但随后我们遇到了不同的列表推导式。使用预期的字段列表 @schema_fields，我们对每个字段都有两部分断言。第一个在第 18 行，是确保该字段存在错误。第二个（第 22 行）是确保它是转换错误。当你编写此类断言时，确保自定义错误消息不同是有帮助的。即使测试输出告诉你错误发生的行，具有相同的消息也会使你在错误断言上意外调试失败变得太容易。最终，我们只关心是否存在转换错误，但由于每个字段的错误存在细微差异，如以下代码示例所示，我们必须绕几个弯。以下片段显示了我们 changeset 的错误：

```
[
  favorite_number: {"is invalid", [type: :float, validation: :cast]},
  date_of_birth: {"is invalid", [type: :date, validation: :cast]},
  last_name: {"is invalid", [type: :string, validation: :cast]},
  phone_number: {"is invalid", [type: :string, validation: :cast]}
]
```

每个错误都有不同的类型，这导致需要多步骤断言。另一种方法是提前处理错误并重新映射列表，使其仅包含字段名和验证类型。Phoenix 提供了一个名为 errors_on 的函数，它将错误重新映射为仅字段名和错误消息（字符串）。它是一个很小的函数，[29] 可以直接复制到你的非 Phoenix 代码库中，但我们不喜欢在不需要时匹配字符串。 本章后面我们将介绍一种替代方案。现在让我们保留两步断言。

在成功测试和错误测试之间，我们已经涵盖了 cast/3 是否存在于 changeset 函数中，并且我们能够对字段定义中的数据类型做出断言。让我们继续测试必填字段。编写下一个测试时，重要的是要知道 Ecto 将 nil、空字符串（“”）和缺失键视为 validate_required/2 的违规。让我们在同一个 describe 块中再添加一个错误用例测试。

```
```elixir
test "error: returns error changeset when required fields are missing" do
  params = %{}

  assert %Changeset{valid?: false, errors: errors} =
    UserBasicSchema.changeset(params)

  optional_params = [:favorite_number]
  expected_fields = @schema_fields -- optional_params

  for field <- expected_fields do
    assert errors[field], "Field #{inspect(field)} is missing from errors."
    {_, meta} = errors[field]

    assert meta[:validation] == :required,
      "The validation type, #{meta[:validation]}, is incorrect."
  end

  for field <- optional_params do
    refute errors[field],
      "The optional field #{field} is required when it shouldn't be."
  end
end
```
```

此测试的格式到现在应该感觉很熟悉了。以下解释了前述代码中的一些值得注意的变化：

- 我们正在传递一个空的参数映射以强制违反 required 验证。
- 我们从列表推导式中移除了可选参数字段（在本例中仅为 :favorite_number）。
- 我们期望的错误类型是 :required 而不是 :cast。

底部还有一个额外的断言，以确保 :favorite_number 没有错误。我们本可以添加断言来保证可选参数上没有错误，但由于我们传递的是空的参数映射，不应有任何方式让未传递的参数上出现错误。

在测试 Ecto 错误的存在时，尝试找出如何以最少的关于正在检查的数据的假设来确保你的测试覆盖其所需的内容。此测试关注 required 错误的存在。我们没有假设我们知道错误列表的大小，这是一种你经常会遇到的做法。虽然表面上看这样的断言可能有帮助，但它会导致脆弱的测试。我们从成功测试知道，如果传入好的数据，一切都正常。此测试确认我们对缺失的必填数据获得了正确的错误。如果我们对 changeset 上的错误做出任何其他假设，我们就有可能在测试失败时出现错误，该错误与测试的关注点无关。这种情况可能发生在 changeset/1 函数中编写了更多逻辑并在 :favorite_number 上插入了不同错误时。错误会存在，但它不是此测试关注的问题。所有单元测试，而不仅仅是 Ecto 相关的测试，都应确保失败是由正在测试的代码中的问题引起的，而不是由测试代码引起的。

我们现在已经编写了测试来覆盖 schema 定义和简单的 changeset 函数。我们的测试检查返回值的各个方面，我们可以确信我们的代码按预期工作。如果你愿意

要对 schema 进行更改，我们所写的测试要么需要更新（如果添加了新字段），要么需要补充新的测试（如果添加了新的验证）。本章后面，我们将对刚才编写的代码和测试进行分支，以将我们的 schema 专门用于输入验证或数据库交互。不过，在此之前，我们将再次审视我们刚刚编写的测试，以识别模式并做出更改，使测试专门针对使用 Ecto.Schema 的代码。

## Refactoring to Increase Test Maintainability

大多数使用 Ecto 的应用程序有不止一个 schema，随着时间的推移，每个 schema 都会增加更多字段或在 changeset 函数中增加更多逻辑。我们将重构现有的测试文件以使代码更易于维护，但这意味着我们将进入一些让部分人感到不安的领域：我们将重构测试使其能够自我更新。我们将在测试方式中加入安全保障，以避免编写自我更新测试时导致大多数人受挫的典型陷阱。

我们要明确指出，我们并不提倡在整个应用程序中全面推广这种测试风格。我们正在处理的代码部分相当可预测且范围有限。我们的 schema 仅包含定义和 changeset 函数。

我们将创建一个新的测试文件 testing_ecto/test/schemas/user_basic_schema_2_test.exs，但它将测试与之前测试文件相同的 schema 代码。我们之前提到过，我们会在测试文件名中添加数字，这会破坏测试文件与应用程序文件名匹配的惯例。尽管我们称之为重构，但最好也创建第二个文件，因为这样更便于跟随本书内容。

## Adding Safety Into Your Schema Tests

打开新的测试文件并添加以下代码。你会看到我们测试的模块与之前的文件相同，即 TestingEcto.Schemas.UserBasicSchema：

```
testing_ecto/test/schemas/user_basic_schema_2_test.exs
```

```
defmodule TestingEcto.Schemas.UserBasicSchema2Test do
  use ExUnit.Case
```

```elixir
alias Ecto.Changeset
alias TestingEcto.Schemas.UserBasicSchema

@expected_fields_with_types [
  {:date_of_birth, :date},
  {:email, :string},
  {:favorite_number, :float},
  {:first_name, :string},
  {:last_name, :string},
  {:phone_number, :string}
]
describe "fields and types" do
  @tag :schema_definition

  test "it has the correct fields and types" do
    actual_fields_with_types =
      for field <- UserBasicSchema.__schema__(:fields) do
        type = UserBasicSchema.__schema__(:type, field)
        {field, type}
      end

    assert MapSet.new(actual_fields_with_types) ==
      MapSet.new(@expected_fields_with_types)
  end
end
end
```

除模块名外，第一个实质性变化是，我们不再使用模块属性 @schema_fields，而是使用名为 @expected_fields_with_types 的属性。该属性包含一个关键字列表，其中包含字段名和我们 schema 的预期字段类型。之前的测试确实让我们确信 schema 为字段定义了正确的类型，但将此信息在测试文件中重复是本次重构中我们承诺提供的安全性的
主要组成部分。我们在为文件添加更多内容后会再回来说明其重要原因。

第 14 行的 describe 块不遵循函数名的惯例，因为我们没有测试公共函数。相反，我们的练习代码有点不寻常。我们不是调用函数，而是构建一个字段名和类型的关键字列表，与我们的 @expected_fields_with_types 模块属性中的类似。为此，我们利用了来自 Ecto.Schema 的两个反射函数调用。如果你还记得，我们第一次在 schema 文件本身中看到反射。我们已经熟悉 __schema__/1（第 18 行）。__schema__/2（第 19 行）返回字段类型的单个原子。我们的生成器只是对 UserBasicSchema.__schema__(:fields) 的一次调用。每次迭代然后调用 UserBasicSchema.__schema__(:type, field)，它返回表示类型的原子，这将与 schema 文件本身中设置的类型完全相同。我们的列表推导返回一个来自 schema 定义本身的字段名和类型的关键字列表，但其格式与存储在 @expected_fields_with_types 中的数据相同。

此测试随后检查测试对字段名和类型的期望（@expected_fields_with_types）是否与文件中实际定义的内容匹配。同样，我们在第一次处理文件时没有这样做，因为我们不需要这样做。我们的测试已在不如此明确的情况下提供了对此的覆盖。需要注意的是，由于我们在断言中使用了 == 进行比较，两个列表必须按相同顺序排列断言才能通过。为了解决这个问题，我们对两个列表都调用 MapSet.new/1。集合是无序的，比较两个集合只考虑元素，这使其非常适合此用例。

此测试的最后一个新特性是在第 15 行添加了测试标签 :schema_definition。此时，我们的测试尚未演进到足以使其有用的程度。本章后面，我们将利用此标签仅运行此测试。

我们说过此测试为我们即将进行的其余重构工作增加了安全性。让我们稍作尝试。注释掉 schema 中的一个字段；比如 field(:first_name, :string)。现在运行你的测试并检查输出。它应该看起来像以下内容。

```
1) test fields and types it has the correct fields and types
(TestingEcto.Schemas.UserBasicSchema2Test)
test/schemas/user_basic_schema_2_test.exs:16
Assertion with == failed
code: assert MapSet.new(actual_fields_with_types) ==
    MapSet.new(@expected_fields_with_types)
left: #MapSet<[{:date_of_birth, :date}, {:favorite_number, :float},
    {:last_name, :string}, {:phone_number, :string}]>
right: #MapSet<[{:date_of_birth, :date}, {:favorite_number, :float},
    {:first_name, :string}, {:last_name, :string},
    {:phone_number, :string}]>
stacktrace:
    test/schemas/user_basic_schema_2_test.exs:23: (test))
```

如果你的测试有彩色输出，你会立即看到 {:first_name, :string} 在预期字段中多余。如果你在 schema 中添加新字段，也会发生类似的失败。我们现在拥有非常快速的验证反馈，表明有些不对劲，这会将我们指向 schema 定义。@expected_fields_with_types 也将在我们即将进行的测试中用作 schema 定义方式的参考。我们刚刚编写的测试将帮助我们确保它始终准确。如果需要向 schema 添加或删除字段，你也只需要更新存储在 @expected_fields_with_types 中的列表即可。

在测试文件的顶部拥有此定义还有一个额外好处。我们在本书中之前提到过，写得好的测试可以作为你代码的文档。在这种情况下，我们更进一步，确保文件顶部有 schema 基础的准确描述。我们不建议仅为了在测试中提供额外的参考信息而这样做，但在测试文件中工作时拥有它会很方便。

现在我们已经保证了其准确性，让我们编写一个测试来利用 @expected_fields_with_types。在继续之前，先取消注释 first name 字段，以便将代码恢复到正确状态。

## Creating a Self-Updating Test

在编写第一个测试之前，我们将在测试文件的底部、在所有 describe 块之后编写一个辅助函数。新函数将命名为 valid_params/1。用以下代码定义它：

```
defp valid_params(fields_with_types) do
  valid_value_by_type = %{
    date: fn -> to_string(Faker.Date.date_of_birth()) end,
    float: fn -> :rand.uniform() * 10 end,
    string: fn -> Faker.Lorem.word() end
  }

  for {field, type} <- fields_with_types, into: %{} do
    {Atom.to_string(field), valid_value_by_type[type].()}
  end
end
```

我们稍后会讨论 Faker 的引入，但首先让我们讨论一下这个函数的作用。该函数的返回值是一个映射，在功能上与我们在本文件上一迭代中的成功测试中拥有的 params 相同。它只是一个有效参数的字符串键映射，但它是根据传递给它的字段和类型列表动态构建的。我们稍后会发展这一概念。现在，我们有一种以可重复的方式获取有效参数列表的方法。提供的值是随机的，因为值本身不应影响测试结果。我们使用匿名函数以便

我们为新字段获取不同的值。这并非严格必要，但能让数据更逼真一些。我们使用一个新库 Faker 来提供部分数据。Faker 的唯一目的就是提供形状逼真的数据。在我们的案例中，我们使用模块 Lorem 来生成随机词。为了让代码保持编译和运行，你需要将 Faker 添加到项目依赖中。在 mix.exs 文件的 deps 部分，你需要添加行 {:faker, "~> 0.13.0", only: [:test, :dev]}。我们将其限制在 test 和 dev 环境，因为我们不打算在生产代码中使用 Faker。我们保留在 :dev 中，因为能够进入 shell 会话 (iex -S mix) 探索功能是很方便的。

现在我们有了辅助函数，可以开始编写测试了。将这个新的 describe 块和测试添加到你的测试文件中。你会注意到它几乎相同，只是我们的列表推导式使用 @fields_and_types 作为生成器（但丢弃类型），并且有效参数来自辅助函数而不是硬编码。

```
```
```elixir
describe "changeset/1" do
  test "success: returns a valid changeset when given valid arguments" do
    valid_params = valid_params(@expected_fields_with_types)

    changeset = UserBasicSchema.changeset(valid_params)
    assert %Changeset{valid?: true, changes: changes} = changeset

    mutated = [:date_of_birth]

    for {field, _} <- @expected_fields_with_types, field not in mutated do
      actual = Map.get(changes, field)
      expected = valid_params[Atom.to_string(field)]
      assert actual == expected,
        "Values did not match for field: #{field}\n expected: #{inspect(expected)}\nactual: #{inspect(actual)}"
    end

    expected_dob = Date.from_iso8601!(valid_params["date_of_birth"])
```

```
assert changes.date_of_birth == expected_dob
end

end
```

现在你已经添加了测试，运行测试文件以确保它通过。一旦通过，让我们探索这次重构的好处。将第一个测试，即断言字段和类型的测试，视为控制。对文件所做的任何更改都需要有意更新该控制。所以，让我们想象我们的代码正在升级，我们将向模式添加一个新字段。在模式定义中添加一个新的中间名字段 (`field(:middle_name, :string)`)。现在再次运行测试，使用 `mix test` `test/schemas/user_basic_schema_2_test.exs`。你会看到不止一个测试失败。

虽然在测试文件还比较新且很小的情况下，很容易找出要关注的测试失败，但随着模式的增长，在这种情况下你可能会遇到更多测试失败。还记得我们添加到测试中的标签吗？由于我们知道模式的字段已更改，现在似乎是隔离专注于模式定义的测试的好时机。重新运行测试，但这次我们也会传递标签，`mix test --only schema_definition test/schemas/user_basic_schema_2_test.exs`。你应该会看到类似以下的单个失败：

```
1) test fields and types it has the correct fields and types
    (TestingEcto.Schemas.UserBasicSchema2Test)
    test/schemas/user_basic_schema_2_test.exs:16
    Assertion with == failed
    code: assert Enum.sort(actual_fields_with_types) ==
        Enum.sort(@expected_fields_with_types)
    left: [{:date_of_birth, :date}, {:favorite_number, :float},
        {:first_name, :string}, {:last_name, :string},
        {:middle_name, :string}, {:phone_number, :string}]
    right: [date_of_birth: :date, favorite_number: :float,
        first_name: :string, last_name: :string, phone_number: :string]
    stacktrace:
        test/schemas/user_basic_schema_2_test.exs:23: (test)
```

我们在注释掉字段时之前见过类似的失败输出，但现在测试告诉我们模式上的字段比测试期望的更多，而不是模式上缺少字段。我们要做的就是更新测试对定义的期望，以包含新字段：

```
@expected_fields_with_types [
{:date_of_birth, :date},
{:favorite_number, :float},
{:first_name, :string},
{:last_name, :string},
{:middle_name, :string},
{:phone_number, :string}
]
```

在为中间名添加字段后，重新运行整个测试文件。所有测试都应该通过。这给我们带来了什么？我们有意添加了一个新字段，我们的测试立即告知它们已过时。根据它们的反馈，我们能够在测试文件中更新一个位置，现在字段和类型的测试通过了，让我们知道测试和模式具有相同的定义。鉴于我们在文件的第一个版本中没有该测试，这并不真的感觉像是一次胜利。价值在于第二个测试，即检查有效参数的成功情况，自动更新了自身。通过更新 @fields_and_types，新字段会自动包含在测试覆盖中。

需要注意的是，如果像 :date_of_birth 一样，数据在 changeset 中被 cast 函数修改，你需要将其添加到被修改值的列表中，并在 changeset 的 changes 中添加对该值的显式断言。虽然在这种情况下它并不完全自更新，但你仍然有测试确保你更新它们的优势，减少了维护工作。

在编写更多代码之前，通过从模式定义中删除中间名字段以及从测试文件中的 @fields_and_types 中删除它，将模式恢复到之前的状态。

首先我们将更新 cast 错误的测试。为此，我们将添加另一个辅助函数，类似于 valid_params/1，但这次称为 invalid_params/1。它会以类似的方式工作，但会返回无法为字段类型转换的值。打开你的测试文件并在其他辅助函数下方添加此函数：

```
```elixir
defp invalid_params(fields_with_types) do
  invalid_value_by_type = %{
    date: fn -> Faker.Lorem.word() end,
    float: fn -> Faker.Lorem.word() end,
    string: fn -> DateTime.utc_now() end
  }

  for {field, type} <- fields_with_types, into: %{} do
    {Atom.to_string(field), invalid_value_by_type[type].()}
  end
end
```
```

该函数显然构造为像 valid_params/1 一样工作，但返回无法转换为匹配类型的值。在添加了它之后，我们现在可以编写用于转换的错误测试的更新版本。你可以编写以下测试，也可以从旧文件中复制测试并更新它：

```
```elixir
1:  test "error: returns an error changeset when given un-castable values" do
-    invalid_params = invalid_params(@expected_fields_with_types)
-    assert %Changeset{valid?: false, errors: errors} =
-        UserBasicSchema.changeset(invalid_params)
5:
-    for {field, _} <- @expected_fields_with_types do
-      assert errors[field], "The field :#{field} is missing from errors."
-      {_, meta} = errors[field]
10:
-      assert meta[:validation] == :cast,
-          "The validation type, #{meta[:validation]}, is incorrect."
-    end
-  end
```
```

就像我们之前的测试一样，此测试几乎与之前的版本相同，但现在它在第 2 行包含对辅助函数的调用以获取坏数据。由于此测试对模式的所有字段进行断言，就像我们已经编写的成功测试一样，我们的工作完成了。欢迎你重复添加新字段到模式的过程，以查看它是否具有我们之前所见的相同结果。如果你这样做，你会看到“字段和类型”测试和成功测试都失败了。我们刚刚编写的测试不会失败，因为它尚未使用新字段，但一旦你更新测试的 @fields_and_types 定义，所有测试都应该通过并且会测试新字段。

我们最后的重构将是 required 字段的错误测试。让我们再次查看此测试的先前版本的代码：

```
test "error: returns error changeset when required fields are missing" do
  params = %{}

  assert %Changeset{valid?: false, errors: errors} =
    UserBasicSchema.changeset(params)

  optional_params = [:favorite_number]
  expected_fields = @schema_fields -- optional_params

  for field <- expected_fields do
    assert errors[field], "Field #{inspect(field)} is missing from errors."
    {_, meta} = errors[field]

    assert meta[:validation] == :required,
      "The validation type, #{meta[:validation]}, is incorrect."
  end

  for field <- optional_params do
    refute errors[field],
      "The optional field #{field} is required when it shouldn't be."
  end
end
```

此测试确保它仅对必需参数测试错误，通过维护一个内部列表来说明哪些参数是可选的。这里

拥有像这样的列表总是有权衡的。当你的测试文件很小且结构良好时，维护该列表并不算太糟糕。当你添加一个可选的新字段时，你需要记住更新列表，但通常是可以管理的。

在我们的重构中，我们可以继续像这样在测试中保留并维护该列表。但与此同时，让我们探索将其移动到文件的顶部，紧跟在 @fields_and_types 属性之后。更新你的属性定义，现在添加一个 @optional 模块属性。

你的属性定义现在应如下所示：

```
@expected_fields_with_types [
  {:date_of_birth, :date},
  {:email, :string},
  {:favorite_number, :float},
  {:first_name, :string},
  {:last_name, :string},
  {:phone_number, :string}
]
@optional [:favorite_number]
```

将测试文件中的可选参数列表移过去有两个目的。最重要的是它位于字段和类型定义正下方。这样一来，当你向 schema 添加可选字段时，你的测试更有可能提醒你它们与 schema 不匹配，并且你需要更新类型和字段。第二个目的更像是一个额外好处，它为你的测试文件增加了一些文档。如果有不同的 changeset 具有不同的必填字段，你会保留多个这样的列表。只要确保为它们提供清晰的名称，这样它们才会有帮助而不是造成混淆。

现在我们已经设置了该模块属性，我们将在重构中利用它。再次说明，你可以从旧的测试文件中获取以下测试并修改它，也可以全部手动输入。将以下测试添加到你的文件中：

```
1: test "error: returns error changeset when required fields are missing" do
  params = %{}
  assert %Changeset{valid?: false, errors: errors} =
    UserBasicSchema.changeset(params)
  for {field, _} <- @expected_fields_with_types, field not in @optional do
    assert errors[field], "The field :#{field} is missing from errors."
    {_, meta} = errors[field]

10:
  assert meta[:validation] == :required,
    "The validation type, #{meta[:validation]}, is incorrect."
  end

  for {field, _} <- @optional do
15:
  refute errors[field],
    "The optional field #{field} is required when it shouldn't be."
  end
end
```

由于测试设置仍然需要一组空参数，更新测试所需的唯一更改是围绕过滤掉可选参数，以便我们只断言每个必填参数都存在错误。我们已更新列表推导式，类似于之前的测试，其中类型被忽略。测试的另一个主要更新是列表推导式现在过滤掉了可选字段，利用了列表推导式的过滤功能和新的 @optional 属性。

运行你的测试，它们都应该通过。我们现在已经重构了测试文件，以两种方式提高了文件的可维护性。第一种是某些测试实际上会随着所测试文件的更改而更新，并且以安全的方式进行。大多数尝试编写自更新测试的方法都未能提供保证其正常工作的机制，因此最终造成的麻烦比消除的更多。我们在这里之所以敢这样做，是因为我们的模式范围非常有限（Ecto schemas），并且我们已经反复编写过足够多的相同文件，对这些模式充满信心。如果你尝试在其他地方应用这种测试风格，请务必非常小心。

这种测试风格的第二个好处是它会在事物发生变化时发出警报，而不是允许新代码未经测试。它非常常见，schema 在事后添加的新字段没有得到正确测试。这些测试不会允许这种情况发生。

现在我们已经重构了 schema 测试，让我们扩展并开始专门化我们的 schema 文件，并更新测试以匹配它。我们将从将 schema 用作数据验证代码开始。

## 创建 SchemaCase 用于共享测试代码

Case 模板在我们识别出测试文件可以从共享代码中受益的模式时非常有用。我们对基本 schema 测试的重构创建了两个辅助函数 valid_params/1 和 invalid_params/1，最好将它们移动到一个公共位置，以便所有 schema 测试重用。我们将创建一个名为 SchemaCase 的 case 模板并将辅助函数移至其中。创建一个名为 testing_ecto/test/schema_case.ex 的新文件并添加 case 模板的基本结构：

```
testing_ecto/test/schema_case.ex

defmodule TestingEcto.SchemaCase do
  use ExUnit.CaseTemplate

  using do
    quote do
      alias Ecto.Changeset
      import TestingEcto.SchemaCase
    end
  end

end
```

目前它非常简单。我们可以将 Ecto.Changeset 别名到 case 模板中，因为所有未来的 schema 测试都需要它。我们包含了 import TestingEcto.SchemaCase；因此当我们添加辅助函数时，任何“使用”该模板的内容都会获得这些函数。我们的最后一步是将两个函数 valid_params/1 和 invalid_params/1 复制到文件中，放在 “using” 块下方。只需确保两个函数都是公共的（使用 def 而不是 defp），因为 import 仅适用于公共函数。case 模板现在已准备就绪，但我们需要确保在运行测试时它会在我们的应用中编译。我们还将对测试文件进行最后一次重构，确保它使用 case 模板并且不再在本地包含代码。

为了确保在应用运行测试时 case 模板可用，让我们修改 mix.exs 文件。打开它并在 project 部分的列表中添加 elixirc_paths: elixirc_paths(Mix.env())：

```
testing_ecto/mix.exs

def project do
  [
    app: :testing_ecto,
    version: "0.1.0",
    elixir: "~> 1.10",
    start_permanent: Mix.env() == :prod,
    elixirc_paths: elixirc_paths(Mix.env()),
    deps: deps()
  ]
end
```

这是在调用一个尚不存在的函数 elixirc_paths/1，因此我们也需要添加该函数。在 mix 文件的 deps 部分上方，添加以下函数头：

```
testing_ecto/mix.exs

1: defp elixirc_paths(:test), do: ["lib", "test"]
2: defp elixirc_paths(_), do: ["lib"]
```

这里最重要的是第 1 行，我们在其中告诉应用程序在测试环境编译时包含 test 目录中的所有文件。这将使 SchemaCase 文件在 :test 环境中可用，同时将其排除在 :dev 和 :prod 环境或以后添加的任何其他环境之外。

现在我们只需要重构测试文件。与其就地更改代码，我们将复制 testing_ecto/test/schemas/user_basic_schema_2_test.exs，并将其命名为 testing_ecto/test/schemas/user_basic_schema_3_test.exs。忽略 describe 块内部的测试，你的文件将如下所示：

```
```elixir
defmodule TestingEcto.Schemas.UserBasicSchema3Test do
  use TestingEcto.SchemaCase
  alias TestingEcto.Schemas.UserBasicSchema

  @expected_fields_with_types [
    {:date_of_birth, :date},
    {:email, :string},
    {:favorite_number, :float},
    {:first_name, :string},
    {:last_name, :string},
    {:phone_number, :string}
  ]
  @optional [:favorite_number]

  describe "fields and types" do
  end

  describe "changeset/1" do
  end
end
```
```

如前所述，请确保重命名你的测试模块，将数字提高到 3（第 1 行）以避免命名冲突。用 use TestingEcto.SchemaCase 替换包含 use ExUnit.Case 和 alias Ecto.Changeset 的两行，如第 2 行所示。现在，由于 SchemaCase 已包含 valid_params/1 和 invalid_params/1 函数，你可以从文件中删除它们。

运行 mix test test/schemas/user_basic_schema_3_test.exs 以验证所有测试是否与之前一样通过。我们现在已将我们认为会在所有测试之间共用的代码移动到一个公共位置，这样我们只需要维护一份函数的副本...

written. We’ll add more common code to our case template later in this chapter.

## 将 Ecto Schema 作为数据验证器进行测试

在 Web 应用中，在控制器中对用户输入进行一些基本验证是非常常见的，这样如果存在易于检测的问题，应用程序就不会在没有成功机会的情况下花费时间做工作（或调用数据库）。我们已经编写的代码会接收输入并将其转换为适当的值，对于无法转换的任何数据或缺少的任何必填参数返回错误。这正是我们所说的那种基本验证。通过一些非常小的更改，我们将更新代码使其作为验证器使用。之后，我们将更新重构后的测试以反映这些更改。

如前所述，我们的代码已经完成了我们想要的工作。现在唯一的问题是接口并不理想。当代码执行成功时，我们的应用代码并不希望接收到一个 changeset。理想情况下，当输入有效时，我们的验证器将返回一个包含 :ok 和包含字段与值的 struct 的元组。当出错时，我们仍然会返回带有错误的 changeset，但放在一个错误元组中。还有一个接口更改我们应该做：重命名函数。由于我们不再返回 changeset，我们将把唯一的公共函数 changeset/1 重命名，以反映代码所做的事情。将 schema 文件复制到路径 lib/schemas/user_validator.ex。我们将展示更新后的文件，然后指出需要进行的更改：

```
testing_ecto/lib/schemas/user_validator.ex

1: defmodule TestingEcto.Schemas.UserValidator do
  - use Ecto.Schema
  - import Ecto.Changeset
  -
5: @optional_fields [:favorite_number]
  -
  - @primary_key false
```

```
- embedded_schema do
- field(:date_of_birth, :date)
  field(:email, :string)
10:
- field(:favorite_number, :float)
- field(:first_name, :string)
- field(:last_name, :string)
- field(:phone_number, :string)
  end
15:
- defp all_fields do
- __MODULE__.__schema__(:fields)
- end
20:
- def cast_and_validate(params) do
- %__MODULE__{}
- |> cast(params, all_fields())
- |> validate_required(all_fields() -- @optional_fields)
|> apply_changes_if_valid()
25:
- end
- defp apply_changes_if_valid(%Ecto.Changeset{valid?: true} = changeset) do
- {:ok, Ecto.Changeset.apply_changes(changeset)}
end
30:
- defp apply_changes_if_valid(%Ecto.Changeset{} = changeset) do
- {:error, changeset}
- end
end
35:
```

第一个需要做的更改是模块名称，将其更新为 TestingEcto.Schemas.UserValidator，如第 1 行所示。希望这不言自明，但这只是提醒我们的命名和文件结构与本书中人为构造的代码相匹配。我们并不推荐这种文件结构，而且绝对不建议将验证器和“normal” schemas 放在同一目录中，因为它们很可能在应用程序的非常不同的部分中使用。

接下来，将文件中公共函数的名字更新为 cast_and_validate，如第 21 行所示。剩下的唯一事情就是更改函数使其返回元组和可能的 struct 而不是 changeset。添加以下两个私有函数头，如第 28 行到第 34 行之间所示。它们期望一个 Ecto.Changeset struct 并对 changeset 中的 :valid? 字段进行模式匹配。你可以看到每个都将响应包装在具有正确状态（:ok 或 :error）的元组中，并且成功的情况调用 Ecto.Changeset.apply_changes/1,[30]，它将返回一个包含所有新值的 TestingEcto.Schemas.UserValidator struct。不过要小心，因为 Ecto.Changeset.apply_changes/1 即使 changeset 无效也会工作，这就是为什么我们对代码进行了分支。

现在剩下的就是在新命名的 cast_and_validate/1 的管道末尾添加我们本地的 apply_changes_if_valid/1。你可以在我们代码的第 25 行看到它。我们现在有一个功能性的验证器。我们需要添加测试。

我们将复制更新后的测试文件 testing_ecto/test/schemas/user_basic_schema_3_test.exs 到新位置 testing_ecto/test/schemas/user_validator_test.exs。我们可以在不做太多工作的情况下更新文件。我们需要做的第一件事是更新正在测试的文件名。文件中 UserBasicSchema 的每个实例都需要替换为 UserValidator。如果您能够进行整体查找和替换，您的测试文件顶部应该看起来像以下代码：

```
testing_ecto/test/schemas/user_validator_test.exs
```

```
defmodule TestingEcto.Schemas.UserValidatorTest do
  use TestingEcto.SchemaCase
  alias TestingEcto.Schemas.UserValidator
```

模块名称（第 1 行）和测试对象的别名（第 3 行）应与您的代码匹配。假设您能够轻松替换所有实例，测试应该能够编译，但运行 mix test 将产生三个失败的测试，因为虽然“fields and types”测试通过反射函数调用代码，但 "changeset/1" describe 块中的三个测试都调用了一个不再存在的函数。让我们更新我们的测试来解决这个问题。更新 describe 块及其中的第一个测试，使其看起来像这样：

```
describe "cast_and_validate/1" do
  test "success: returns a valid changeset when given valid arguments" do
    valid_params = valid_params(@expected_fields_with_types)
    {:ok, result} = UserValidator.cast_and_validate(valid_params)
    assert %UserValidator{} = result
    mutated = [:date_of_birth]
    for {field, _} <- @expected_fields_with_types, field not in mutated do
      assert Map.get(result, field) == valid_params[Atom.to_string(field)]
    end
    expected_dob = Date.from_iso8601!(valid_params["date_of_birth"])
    assert result.date_of_birth == expected_dob
end
```

将 describe 描述更改为 cast_and_validate/1（第 1 行）。接下来，我们需要以两种方式更新调用代码的行（第 5 行）。它应匹配到 {:ok, result}，并且应调用 UserValidator.cast_and_validate/1 来调用代码。下一行（第 6 行）应更新为断言结果是预期的 struct 类型，%UserValidator{}。这两个行本可以合并，但我们

由于行长限制而分开。如果这是您偏好的风格，请随意将它们合并在一起。

还有两个地方需要更新测试代码。changes 应该在第 10 行和第 14 行替换为 result。测试现在应该能够运行并通过。让我们通过运行 mix test test/schemas/user_validator_test.exs: 后面立即跟上测试中间的行号（例如 mix test test/schemas/user_validator_test.exs:39）来试试。这将只运行一个测试，跳过我们已知不工作的测试。这在这里不会影响我们。使用测试中间的行号而不是测试开始的行号的原因是，随着您更改文件，您使用的行号更有可能继续引用相同的测试。如果您使用测试第一行的行号，在文件上方添加一行代码就会破坏该引用。这在您必须向文件中添加调试代码时特别有用，这可能会将测试的开始移动到后面的行号。

测试应该通过，但您仍会看到一些警告，告诉您有一个未使用的别名，并且正在调用一个私有或未定义的函数。让我们通过更新下一个测试来清除这些警告，该测试关注无法转换的值的错误。

测试应更新如下：

```
```elixir
1: test "error: returns an error changeset when given un-castable values" do
-   invalid_params = invalid_params(@expected_fields_with_types)
-
-   assert {:error, %Changeset{errors: errors}} =
5:         UserValidator.cast_and_validate(invalid_params)
-
-   for {field, _} <- @expected_fields_with_types do
-     assert errors[field], "The field :#{field} is missing from errors."
-   {_, meta} = errors[field]
10:
```



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

### The database for TestingEcto.Repo has been created
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

```
add(:last_name, :string, null: false)
add(:phone_number, :string, null: false)
timestamps()
end

create(unique_index(:users, [:email]))
end
end
```

这里有几个细节需要指出。首先，我们在第 5 行告诉迁移跳过为表添加自增 primary_key，然后在第 6 行添加主键。我们的表将使用 UUID 作为主键。在稍后更新模式时我们需要记住这一点。我们还在所有列上使用了 null: false 选项，除了 favorite_number，我们在模式中将其设为可选。在我们的 change/0 中最后需要注意的是，随着 timestamps()（第 14 行）的添加，我们将添加两个当前不在模式中的字段，inserted_at 和 updated_at。这些也将纳入对模式的更新。

在表定义下方，我们还在 email 列（第 17 行）上创建了一个唯一约束。在测试中开始与数据库交互后我们会处理这个问题。

现在我们需要运行迁移以确保它能正常工作。我们在这里将重点关注测试数据库，因此请通过运行 MIX_ENV=test mix ecto.migrate 来设置正确的 mix 环境。这应该会产生成功的反馈：

```
15:36:12.729 [info] == Running 20200518212741
TestingEcto.Repo.Migrations.AddUsersTable.change/0 forward

15:36:12.730 [info] create table users
```

15:36:12.738 [info] == Migrated 20200518212741 in 0.0s

返回登录到 Docker 容器的会话，其中 Postgres cli 已打开。现在你应该能够运行 \dt 来查看 users 和 schema_migrations 表。运行 \d users 并验证你是否看到以下表定义：

Table "public.users"

| 列 | 类型 | 排序规则 | 可空 | 默认值 |
|---|---|---|---|---|
| id | uuid |  | 非空 |  |
| date_of_birth | date |  | 非空 |  |
| email | character varying(255) |  | 非空 |  |
| favorite_number | double precision |  |  |  |
| first_name | character varying(255) |  | 非空 |  |
| last_name | character varying(255) |  | 非空 |  |
| phone_number | character varying(255) |  | 非空 |  |
| inserted_at | timestamp without time zone |  | 非空 |  |
| updated_at | timestamp without time zone |  | 非空 |  |

索引：
"users_pkey" PRIMARY KEY, btree (id)

假设你的表与我们的一致，我们就可以继续了。

## 更新我们的模式以配合数据库工作

我们现在准备更新模式以匹配此表。从技术上讲，我们将更新模式的副本，以便在不破坏其他测试文件的情况下进行更改。将 testing_ecto/lib/schemas/user_basic_schema.ex 复制到名为 testing_ecto/lib/schemas/user_database_schema.ex 的新文件中。稍后我们将复制测试文件以匹配。

现在，我们将更新模式文件，使其如下所示：

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

首先，将 user_database_schema.ex 中的模块名称更改为 TestingEcto.Schemas.UserDatabaseSchema，如第 1 行所示。我们需要在文件中设置一些 Ecto Schema 选项（使用模块属性），使其与我们在迁移中设置的时间戳和主键选项兼容。向文件中添加两个选项 @timestamps_opts type: :utc_datetime_usec（第 5 行）和 @primary_key {:id, :binary_id, autogenerate: true}（第 6 行）。如果这是一个真正的应用程序，这些选项可以设置在通用的模式模板中。如果你还记得，我们的基本模式在模式定义正上方设置了 @primary_key false。现在它被更新以对应数据库表，我们需要将其删除。通过删除它，并将主键选项设置为 :binary_id，我们的模式现在多了一个类型为 :binary_id 的字段 :id。

目前，我们的 changeset 函数仍然是一个通用的 changeset 函数，因此 presumably 将用于创建新用户。因此，虽然 :id 可能在更新 changeset 中需要，但当用于创建时，我们需要将其设为可选字段。稍后我们会将此函数拆分为两个更具体的 changeset 函数，但为了保持示例简洁，现在我们保留一个简单的函数。请注意，我们更新了可选字段的模块属性以包含 :id（第 7 行）。

此时，我们的模式并非设计为引用数据库表，因为它使用 embedded_schema 而不是 schema。我们可以将定义的那一行更新为 schema "users" do（第 9 行），模式现在将被设置为引用我们的 “users” 表。我们偏离了模块名称与表名称匹配的惯例，因为我们处理的是同一文件的多个不同版本。通常，我们会将模式命名为 “User”，表命名为 “users”。在定义块底部，添加 timestamps()，如第 17 行所示，以将 :inserted_at 和 :updated_at 添加到模式中。

我们将 email 字段设为唯一，因此需要更新 changeset 以考虑这一点，在第 28 行添加对 unique_constraint/2 的调用。就这样。我们的模式文件现在已设置好以引用数据库表。接下来，我们将更新测试以考虑新字段。完成后，我们将查看如何测试 unique_constraint 代码。

## 更新我们的测试以包含数据库字段

我们已设置模式 UserDatabaseSchema 以对应数据库表，但这意味着有三个新字段（:id、:inserted_at 和 :updated_at）。为了进行这些更改，我们将再复制一次基本模式测试文件的最后一个迭代，testing_ecto/test/schemas/user_basic_schema_3_test.exs。复制它，命名为 testing_ecto/test/schemas/user_database_schema_test.exs。让我们将测试名称更新为 TestingEcto.Schemas.UserDatabaseSchemaTest，并将所有对 UserBasicSchema 的引用替换为 UserDatabaseSchema，包括别名。

使用正确的模块名称，你的测试文件应该能够运行。运行 mix test --only schema_definition test/schemas/user_database_schema_test.exs，你应该会看到一些错误，因为我们添加了新字段，但测试中字段和类型的定义（存储在 @expected_fields_with_types 中）尚未更新。我们应该查看的测试将有以下输出：

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

我们熟悉此输出并知道该怎么做。我们需要在测试的模式定义中添加 :id、:inserted_at 和 :updated_at。将你的 @expected_fields_with_types 更新为包含新字段：

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

之后，单个测试应该会通过，但如果我们运行整个文件会怎样？运行 mix test test/schemas/user_database_schema_test.exs 现在会产生我们尚未见过的错误。这些错误将针对成功测试和类型转换错误测试，两者的输出都将类似于以下内容：

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

理解这些错误的關鍵是第 4 行，我们看到代码期望一个函数，但得到的是 nil。结合堆栈跟踪的顶部（第 7 行），我们可以了解是什么坏了以及代码中发生的具体位置。我们的辅助函数，

valid_params/1 出现故障，因为它没有为我们新引入的数据类型 :binary_id 和 :utc_datetime_usec 定义函数。第二个测试也几乎因为相同原因会失败：invalid_params/1 也缺少这些定义。

我们可以采用两种方法之一来解决这些失败：我们可以只添加我们需要的类型，或者我们可以遍历所有 Ecto 原始类型并为每种类型创建返回有效和无效数据的函数。[32] 你几乎永远不需要全部类型，而且你的应用程序中可能会出现自定义 Ecto 类型。[33] 由于我们无法列出一个确定的清单，因此在需要时添加新定义更有意义。好消息是，由于代码在 case 模板中，我们只需添加一次，就能让使用相同模板的任何测试都可以使用。

打开 SchemaCase 文件（testing_ecto/test/schema_case.ex）并使用以下新定义更新你的两个辅助函数：

```
testing_ecto/test/schema_case.ex

def valid_params(fields_with_types) do
  valid_value_by_type = %{
    date: fn -> to_string(Faker.Date.date_of_birth()) end,
    float: fn -> :rand.uniform() * 10 end,
    string: fn -> Faker.Lorem.word() end,
    utc_datetime_usec: fn -> DateTime.utc_now() end,
    binary_id: fn -> Ecto.UUID.generate() end
  }

  for {field, type} <- fields_with_types, into: %{} do
    {Atom.to_string(field), valid_value_by_type[type].()}
  end
end

def invalid_params(fields_with_types) do
  invalid_value_by_type = %{
    date: fn -> Faker.Lorem.word() end,
    float: fn -> Faker.Lorem.word() end,
    string: fn -> DateTime.utc_now() end,
    utc_datetime_usec: fn -> Faker.Lorem.word() end,
    binary_id: fn -> 1 end
  }

  for {field, type} <- fields_with_types, into: %{} do
    {Atom.to_string(field), invalid_value_by_type[type].()}
  end
end
```

现在我们已经更新了这些辅助函数，还有一个测试未通过：关于必填字段的测试抱怨 :id 字段没有错误。由于它不应该是必填的，我们只需更新 `test/schemas/user_database_schema_test.exs` 中的模块属性 `@optional`，以包含 `:id`：

```
@optional [:id, :favorite_number]
```

在我们对可选字段进行更新后，我们的 schema 已设置好以反映我们的表，并且我们的测试也已更新以匹配。我们有一个设计问题需要承认：我们的 changeset 函数要求传入 ID。这是因为我们使用的是单个 changeset 函数，而不是为不同目的专门化的函数，例如创建新用户和更新现有用户。我们很快会解决这个问题，但现在，让我们回顾一下我们所取得的成就。我们有一个经过彻底测试的用户 schema，它设置好可与“users”表一起工作。我们添加了新字段，测试让我们知道在哪里进行更新。我们已准备好开始在测试中针对数据库工作。

## Testing Your Schema Through Database Calls

虽然我们已经定义了一个不错的基础 schema，但我们尚未涵盖 changeset 函数中你会找到的一类逻辑的子集——需要与数据库交互以验证其是否正常工作的代码。我们迄今为止测试的验证为我们提供了一个模式，可以用于 cast/3 和 validate_required/2 之外的其他验证。你的代码可以调用并以类似方式测试的函数包括（但不限于）validate_inclusion/4、validate_length/3 和 validate_subset/4。在数据库中测试起来更容易的非常常见的情况是数据库列上的唯一约束。在本节中，我们将向我们的 schema 和表添加一个新字段，包括此类约束。我们将回顾为什么将数据库引入测试是有意义的，然后我们将更新测试以展示如何行使该约束。

你会记得，当我们创建迁移时，我们在 email 字段上添加了唯一约束。我们还在添加 unique_constraint/2 时更新了 changeset/1 函数以包含此逻辑的逻辑。unique_constraint/2 是我们接下来要关注的内容。在测试它之前，我们应该谈谈该函数的作用。

唯一约束由数据库处理，而不是由我们的 changeset 函数处理。那么为什么我们在 changeset 函数中调用 unique_constraint/2 呢？该函数在 changeset 中添加一个“注释”，以便 Ecto 知道如果数据库返回 email 字段唯一约束的错误，而不是引发错误，它应该将 Ecto 约束错误添加到 changeset 错误列表中。如果你检查从 changeset/1 返回的 changeset，你将看不到它，因为 Ecto 对 Inspect 协议有自定义实现。[34] Changeset 结构体下有一个“隐藏”部分

键为 :constraints。如果你深入挖掘我们返回的 changeset 上该键的值，你会看到以下代码：

```
[
  %{
    constraint: "users_email_index",
    error_message: "has already been taken",
    error_type: :unique,
    field: :email,
    match: :exact,
    type: :unique
  }
]
```

这个映射是我们上一段提到的“注释”。当 Ecto 从数据库收到 unique_constraint 错误时，它会检查它正在执行的 changeset 是否具有匹配的约束。如果有，它会使用该映射中的信息生成错误。我们可以在两种方式之一测试该逻辑是否在我们的函数中：测试 changeset 上的返回值或尝试插入 changeset 并测试预期的错误。虽然第一个选项对于保持测试简单并保持一切纯粹函数式很有吸引力，但除非维护该测试的每个人都理解为什么这是测试唯一约束代码的有效方式，否则它会有些难以理解其用途。

撇开前一个测试的“哲学纯粹性”不谈，使用第二个选项，即尝试将数据插入数据库，对我们来说很容易，并且测试最终看起来会与我们之前的测试非常相似。一致性使维护和理解变得更容易。

我们将在测试文件 test/schemas/user_database_schema_test.exs 中 "changeset/1" 的 describe 块底部添加另一个错误测试。打开文件并添加以下测试：

```
1: test "error: returns error changeset when an email address is reused" do
-   {:ok, existing_user} =
-   valid_params(@expected_fields_with_types)
-   |> UserDatabaseSchema.changeset()
5:   |> TestingEcto.Repo.insert()
- 
-   changeset_with_repeated_email =
-   valid_params(@expected_fields_with_types)
-   |> Map.put("email", existing_user.email)
|> UserDatabaseSchema.changeset()
10: 
- 
-   assert {:error, %Changeset{valid?: false, errors: errors}} =
-       TestingEcto.Repo.insert(changeset_with_repeated_email)
- 
    assert errors[:email], "The field :email is missing from errors."
15: 
-   {_, meta} = errors[:email]
- 
-   assert meta[:constraint] == :unique,
-       "The validation type, #{meta[:validation]}, is incorrect."
    end
20:
```

让我们检查这个测试，看看它与我们之前的所有测试有何不同。我们的设置是在数据库中创建一个现有用户，实际上在第 5 行调用了 TestingEcto.Repo.insert/1。我们需要此插入，因为我们试图触发数据库中唯一约束冲突的错误。

设置的第二部分是创建一个 changeset，其中参数有效，除了电子邮件地址，我们用现有用户的电子邮件地址覆盖它（第 9 行）。此测试的练习步骤不正常。被测试的代码，changeset/1 函数，已经被调用。相反，我们使用第二次数据库插入调用来触发我们正在寻找的错误。你可以看到

第 12 行：模式反映了来自 Repo.insert/1 的预期返回值，而不是我们的 changeset/1。

我们将 errors 绑定到变量 errors，然后断言看起来很多都像我们之前的测试。因为只有一个字段应该反映此错误，所以我们没有为断言使用列表推导。还值得指出的是，我们错误上的元数据是 :constraint 而不是像我们之前的测试那样的 :validation。

这个测试没有像我们之前的测试那样编写为自我更新或捕获 schema 更改。我们的测试没有办法使用反射来知道哪些字段应该处理唯一约束。实际定义在数据库本身中，而不是 schema 定义的一部分。Ecto.Schema 中的反射函数仅适用于定义本身。这意味着，虽然我们 schema 的基本功能、定义以及 cast 和 required 验证可以相当动态，但当我们想要更新具有唯一约束的字段时，我们需要确保将它们添加进去。如果我们愿意，这个测试可以更新为使用列表推导。

我们应该提到这个测试的几个注意事项。第一个是，将用户插入数据库的代码利用了我们自己的应用程序代码 UserDatabaseSchema.changeset/1。一般来说，在设置步骤中避免使用你自己的应用程序代码是一个很好的做法，因为与该特定测试无关的代码问题可能会导致大量误导性的测试失败。我们将在下一章中介绍一种替代方案，使用一个名为 factory 的辅助模块。

这个测试的第二个问题是它缺少测试的一个重要阶段：清理。幸运的是，每个测试都不需要保留

跟踪它需要清理的内容。Ecto 为我们提供了一种方法。

## 设置 Sandbox 模式

我们的数据库会保留状态，而目前这个测试会在数据库中留下新的行。短期内，这不太可能造成问题，因为每次运行测试时我们都会使用一个新的随机字符串。从长远来看，测试运行之间存在持久数据会增加因不可预见的数据冲突而导致测试失败的可能性。如果在不同的测试运行中恰好选中了相同的随机字符串，测试会在第一个插入的用户上失败，而该用户本应是设置的一部分。

在测试中操作数据库是残留状态变更导致问题最常见的地方之一。我们通过使用随机化数据避免了唯一约束测试中的问题。但即便如此，每次连续的测试运行都会在数据库中留下新的一行。每个测试在已知良好的环境中运行效果最佳，因为只有这样它们才能保证自己的设置完整且可靠。如果测试无法假设一个干净的工作环境，我们就需要编写更多防御性代码来检查环境中是否存在数据，而不是仅仅创建测试所需的内容然后继续进行。这种方法既会减慢编写测试的速度，因为需要更多代码，也会减慢测试本身的速度。虽然从时间角度来看，多做一两次数据库检查并不是什么大问题，但如果你编写了足够多的测试，它们会累积起来。在隔离的测试中，我们只需专注于确保测试清理了它创建的内容。幸运的是，Ecto 为我们提供了一个非常好的解决方案，使得每个单独的测试不必跟踪并清理自身：Ecto Sandbox。[35]

Ecto Sandbox 为我们的测试提供了两个关键价值。第一个是通过管理连接池：当正确配置时，并发测试可以在不共享数据库状态的情况下运行。这意味着可以利用并发测试的速度提升，而无需我们试图避免的那种防御性编程。设置起来有点复杂，但它运行良好。Ecto Sandbox 提供的第二个价值是每个数据库连接都在数据库事务中运行，并且当测试完成时，事务会被回滚。这意味着测试完成后数据库的状态不会发生变化；换句话说，Ecto Sandbox 为我们提供了数据库更改的自动清理。

我们将设置 Sandbox，选择一种适用于单元测试有限范围的模式。我们将在关于测试 Phoenix 的章节中讨论其他模式，在这些模式中，同一个 Sandbox 需要被多个进程共享。在你的案例模板文件 testing_ecto/test/schema_case.ex 中，我们将添加一个 setup 块，该块将被使用此案例模板的所有测试使用。在该文件中，在 using 块下、在 valid_params/1 函数之上，添加一个如下所示的 setup 块：

```
testing_ecto/test/schema_case.ex

setup do
  Ecto.Adapters.SQL.Sandbox.mode(TestingEcto.Repo, :manual)
end
```

现在，使用此案例模板的每个测试都将在测试执行之前作为设置步骤的一部分运行此代码。这也适用于不引起任何数据库交互的测试。幸运的是，这是一个非常快速、轻量的调用，不会对你的测试套件的整体运行时间产生显著影响。

为了让 Sandbox 在我们的测试中可用，我们需要确保 config/test.exs 中的配置将 TestingEcto.Repo 设置为使用 Sandbox。如果你使用的是我们随应用程序副本提供的那个文件，你应该已经准备就绪，但我们会指出这一步，以确保你知道如何设置它。在你的测试配置的 Ecto 部分，你需要告诉 Ecto 你的数据库连接池将使用 Sandbox。它至少应包含以下行：

testing_ecto/config/test.exs

```
config :testing_ecto, TestingEcto.Repo,
  database: "testing_ecto_test",
  pool: Ecto.Adapters.SQL.Sandbox
```

将 Sandbox 设置为 :manual 使每个测试能够请求自己的 sandbox 连接。该连接在单个测试的整个生命周期内保持不变。换句话说，如果我们的测试设置插入了一条记录，那么在整个执行阶段对数据库的调用过程中，该记录稍后仍会存在。这可能感觉很明显，但实际上在其他 sandbox 模式之一中并不能保证。主要限制是每个 sandbox 实例只能在利用单个进程的测试中使用。在我们的情况下，我们的测试进程是唯一运行的进程，因为被执行的代码全部在测试进程内执行。

我们需要更新我们的测试，在测试开始时显式地从 Sandbox 请求一个连接。这样一来，不使用数据库的测试就不需要任何额外资源。在你的唯一约束错误测试中，更新你的测试，使其第一行读取 Ecto.Adapters.SQL.Sandbox.checkout(TestingEcto.Repo)。这告诉 Sandbox 提供一个与测试进程 ID (PID) 关联的隔离数据库连接。

这就是我们需要做的全部，以使我们的测试不会在数据库中留下数据。此测试的任何后续运行都不会留下数据，但请记住我们至少已经在没有 Sandbox 的情况下运行过一次测试。因此，在继续之前我们应该重置测试数据库。关闭 Docker 容器（运行 make down）是可行的，因为我们的 docker-compose.yml 没有在任何地方挂载磁盘用于持久化。不过重新启动它时，你需要再次创建并迁移你的数据库。另一个选项是按顺序运行以下命令：MIX_ENV=test mix ecto.drop、MIX_ENV=test mix ecto.create，最后 MIX_ENV=test mix ecto.migrate。我们在 Makefile 中包含了一个目标，通过在命令行运行 make reset.test 来完成所有这些操作。好消息是，现在我们使用 Sandbox 后，就不应该再需要这样重置了。

## 总结

本章我们专注于测试 Ecto schemas。我们以此主题作为访问案例模板和辅助函数的方式，并涉及了 Ecto Sandbox。我们还深入探讨了构建测试，使其要么安全地自我更新，要么在测试文件与模式定义不同步时通知你，这两者都减少了维护测试文件的工作量。

在下一章，我们将学习如何测试查询并利用工厂模式来加快编写测试并使健壮的设置更简单。

## 脚注

[28] https://hexdocs.pm/ecto/Ecto.Schema.html#module-reflection

[29] https://github.com/phoenixframework/phoenix/blob/cc261a67a83649555841b92c3cbc1df024888cc8/installer/templates/phx_ecto/data_case.ex#L48

[30] https://hexdocs.pm/ecto/3.4.4/Ecto.Changeset.html#//apple_ref/JavaScript/apply_changes%2F1

[31] https://docs.docker.com/get-started/

[32] https://hexdocs.pm/ecto/Ecto.Schema.html#module-primitive-types

[33] https://hexdocs.pm/ecto/Ecto.Type.html

[34] https://github.com/elixir-ecto/ecto/blob/87ce8e3223a1c8a92c3cbc1df024888cc8/lib/ecto/changeset.ex#L2935

[35] https://hexdocs.pm/ecto_sql/Ecto.Adapters.SQL.Sandbox.html
