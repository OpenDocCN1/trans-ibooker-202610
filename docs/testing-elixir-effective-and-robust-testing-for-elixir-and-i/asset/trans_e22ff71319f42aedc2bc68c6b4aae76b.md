
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
