
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
