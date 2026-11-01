
作为一般指导原则，按函数对测试进行分组是一个不错的起点，如以下示例所示：

```
unit_tests/misc/describe.exs

defmodule YourApp.YourModuleTest do
  use ExUnit.Case

  describe "thing_to_do/1" do
    test "it returns :ok, calls the function if the key is correct"
    test "it does not call the function if the key is wrong"
  end
end
```

你可能已经注意到，描述中包含 /1 来表示元数（函数接受的参数数量）。这是一个风格选择。你可以根据自己认为最合乎逻辑的方式对测试进行分组。在本书后面，当我们处理集成测试时，你可能会按它们所访问的端点等进行分组。其目的是使测试更易于阅读，以便你（以及你的协作者）可以决定什么最合理。指导你的设计的一个因素是你的函数是否具有共同的设置。

## 设置块

虽然单个测试可以包含自己的设置代码，但有时你会发现自己一遍又一遍地编写相同的设置。这种情况是一个考虑使用设置块的机会。设置块中的代码将在其作用域内的每个测试之前执行。由于代码与测试设计中的阶段名称相同，我们将可执行代码称为设置块，而将测试阶段称为 setup。

设置块需要返回一个对后续内容有意义的值。如果设置是为了在其他地方设置状态，并且后续测试中没有任何值有用，则返回值可以简单为 :ok。更常见的是，设置块的返回值会传递给它前面的测试。随着 Elixir 和 ExUnit 的成熟，常见的做法是让 setup 返回一个可在后续测试中使用的值映射。在测试纯函数式代码时，这通常是你会使用设置块的唯一时间，因为没有共享状态需要设置。让我们看一个设置块的示例，其中没有状态被改变，但我们决定不想为每个测试重新定义相同的匿名函数。

unit_tests/misc/callbacks.exs

```elixir
setup do
  function_to_not_call = fn ->
    flunk("this function should not have been called")
  end

  function_to_call = fn -> send(self(), :function_called) end

  %{bad_function: function_to_not_call, good_function: function_to_call}
end
```

请记住，当你将逻辑提取到设置块中时，单个测试的可读性会稍有下降。设置中的每个值都不会在每个测试中使用，但如果你将某个值提取到设置块中，它应该在多个测试中使用。在前面的示例中，坏函数和好函数几乎不可能在同一测试中使用，因为一个用于成功情况，另一个用于失败情况，但由于每个函数都将被多个测试使用，将它们都提取到公共设置中是值得的。

在测试中接受来自 setup 的值需要对测试定义进行轻微修改：

unit_tests/misc/callbacks.exs

```elixir
test "does not call the function if the key is wrong",
  %{bad_function: bad_function} do
  assert {:error, _} =
    YourModule.thing_to_do(:bad_first_arg, bad_function)
end
```

在测试中添加一个作为第二个参数的模式允许你提取 setup 传递的任何内容。如果传递的东西太多，以至于解构映射使测试难以阅读，你始终可以使用一个变量，并在需要时从中提取所需内容。

```
unit_tests/misc/callbacks.exs

test "does not call the function if the key is wrong", context do
  assert {:error, _} =
    YourModule.thing_to_do(:bad_first_arg, context.bad_function)
end
```

## 选择要移入公共设置的内容

![](img/c08a369b9a1934f7c9c3b485727eca8e_40_0.png)

很容易在应该移入设置的代码上过度延伸。如果将太多内容移入公共块，单个测试的目的就难以理解。你可以通过选择描述从设置块接收内容的名称来避免这种情况。不要使用 user，你的变量可以命名为 user_with_completed_profile。有时一个长名称可以为后来者节省大量工作。

你的 setup 的作用域由测试的逻辑分组决定。如果你的 setup 在 describe 内部，它将在该 describe 内部分组的测试之前运行。如果它位于测试文件的顶部——在任何 describe 之外——它将在该文件中的所有测试之前运行。

你的设置可以层层构建。如果整个文件都有一个共同的 setup，但你也需要为 describe 块设置一个 setup，则 describe 中的 setup 可以通过向测试的参数添加模式来接受从更高层 describe 传递给它的值。

```elixir
setup context do
  additional_value = "example value"
  Map.merge(context, %{additional_value: additional_value})
end
```

添加模式可能有帮助，但要小心选择应移入更高层设置块的内容，因为你开始将理解每个测试所需的逻辑分散到文件的多个不同区域。能够在无需查看文件或代码库中不同位置的情况下阅读和理解单个测试，具有很大的价值。

稍后我们将讨论 test cases，这又是帮助减少代码重复的一种结构。它们提供了一个额外的设置层，这次是在多个测试文件之间。由于该设置是在当前测试文件之外定义的，花时间考虑如何组织测试以确保一切易于跟进绝对值得。即使没有嵌套的 describe 块，如果我们在组织测试时不考虑这种平衡，也很容易接近可读性和维护性的噩梦。

setup 的另一个功能可以帮助在减少单个测试或多个设置中的重复的同时保持测试的清晰度：setup 可以接受函数名称或函数名称列表，而不是块。这些函数应遵循一种模式，接受现有上下文（由之前的设置块或函数传递的数据）并返回更新后的上下文，以便它们可以被管道传输。如以下示例所示，with_authenticated_user/1 期望从之前的设置函数接收的数据映射，向该映射添加更多数据，然后返回该映射。

```
unit_tests/misc/setup_with_helper_functions.exs

def with_authenticated_user(context) do
  user = User.create(%{name: "Bob Robertson"})
  authenticated_user = TestHelper.authenticate(user)

  Map.put(context, :authenticated_user, authenticated_user)
end
```

如果你定义了遵循该模式的函数，你可以向 setup 传递一个要执行的函数列表：

```
unit_tests/misc/setup_with_helper_functions.exs

setup [:create_organization, :with_admin, :with_authenticated_user]
```

函数按列出的顺序执行。这允许你在不同的设置作用域中重用相同的逻辑，并使设置逻辑更接近（在文件中）其使用位置。此功能旨在消除对嵌套 describe 块的需求。随着使用的深入，你会发现它是一个相当优雅的解决方案。

虽然 setup 是大多数场景下的正确解决方案，但还有几个额外的函数可以帮助组织测试。

## setup_all

通常你需要一个设置步骤，但不需要在每个测试之前重复执行。如果你的测试在这些步骤仅对整个文件执行一次时也能正常工作，你可以使用 setup_all。除了运行频率外，setup_all 与 setup 工作方式相同，这意味着它可以用函数列表、代码块或上下文模式和代码块来使用。返回值应与你为其他设置函数设置的模式匹配。
