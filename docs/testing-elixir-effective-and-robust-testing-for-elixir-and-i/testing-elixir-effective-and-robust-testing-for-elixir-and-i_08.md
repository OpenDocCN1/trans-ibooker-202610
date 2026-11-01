# Chapter 7

## Property-Based Testing

即使是最好的设计单元测试，也仅限于你可以对代码投放的输入数量以及你能想到的输入数量。你可以使用某种枚举包装器围绕测试，或者尝试想出边缘情况，但你仍然仅限于你实际可以输入的输入，往往因为，嗯，你从未想过它们而遗漏了有问题的输入。

在本章中，你将学习一种新的测试技术，称为*基于属性的测试*。基于属性的测试通过生成随机输入来为你的代码提供输入，从而将*随机性*引入你的测试。这增加了你对代码的信心，并且可以通过生成奇怪的边缘情况来发现讨厌的错误。基于属性的测试还迫使你以不同的方式思考接受的输入和代码的属性，通常有助于设计和实现。

本章的目标是向你介绍基于属性测试的基础知识以及何时以及如何将其集成到你的测试套件中。你还将学习一个名为 stream_data,[47] 的特定工具，由本书的作者之一 Andrea 编写的 Elixir 中相当常见的基于属性的测试框架，但这里讨论的概念和思想适用于你在野外找到的大多数框架。

让我们从一个轶事开始本章，我们认为它捕捉到了基于属性测试的强大和有用性。

### Property-Based Testing in Practice in the Elixir Standard Library

一段时间前，在 Elixir 语言存储库中打开了一个问题，报告了 String.replace/3.[48] 的问题。String.replace/3 是一个函数，它接受一个主题字符串、一个模式字符串和一个替换字符串。函数的工作是将主题字符串中的模式字符串替换为替换字符串。问题报告称，当给定空字符串 "" 作为模式字符串（第二个参数）时，函数会失败。换句话说，用其他内容替换任何字符串中的空字符串都不会工作。很快有人指出，空字符串作为模式字符串的行为很难定义：一方面，你会认为空字符串不包含在任何字符串中，所以你永远不应该替换它，下面的代码应该失败或返回主题字符串不变：

```
String.replace(any_string, "", "some other string")
```

另一方面，如果你尝试使用空字符串作为模式字符串但也作为主题字符串怎么办？也就是说，如果你替换空字符串中的空字符串呢？

```
String.replace("", "", "x")
```

直觉上，你应该得到 "x"，对吧？为了解决这一讨论，Elixir 核心团队最终想出了 String.replace/3 的一个属性。代码的属性是无论给该代码什么输入都“成立”（保持为真）的东西。在这种情况下，String.replace/3 的属性可能是无论主题字符串和要替换的字符串如何都为真的东西。忽略这里的 check all 语法，我们将在本章后面介绍它，只关注语义。

```
check all subject <- string(:printable),
    replacement <- string(:printable) do
  assert String.replace(subject, subject, replacement) == replacement
end
```

该属性指出，对于作为主题字符串的任何随机字符串和作为替换字符串的任何随机字符串，如果你使用相同的字符串作为主题字符串和模式字符串，则 String.replace/3 应该返回替换项。一旦运行测试来检查此属性，空字符串就被发现使属性不成立。由于 Elixir 团队确信 String.replace/3 应该具有此属性，这意味着 String.replace/3 的实现实际上是错误的。String.replace/3 已被修复以处理空模式字符串，使属性成立的方式。

```
iex> String.replace("ELIXIR", "", ".")
".E.L.I.X.I.R."
```

```
iex> String.replace("ELIXIR", "", "")
"ELIXIR"
```

关于替换字符串的故事只是现实世界中用于驱动代码设计并验证其按预期行为的属性的一个例子。然而，它展示了以属性为思维方式思考代码并在测试时生成随机数据来验证这些属性的强大功能。在本章的其余部分，我们将学习更多关于基于属性测试的技术和工具。我们将首先查看我们到目前为止编写的测试，以及它们从基于属性测试的角度如何契合。

### Example-Based Tests

在编写测试时，我们通常编写一些我们可以称为*基于示例*的测试。在本节中，我们将从我们提供给代码的输入的角度重新审视它们。我们将定义术语来识别基于示例的测试，这将帮助我们稍后将它们与基于属性的测试进行比较。

让我们从一个例子开始。假设我们想要测试 Enum.sort/1 函数是否正确排序列表。我们可以称以下为基于示例的测试，因为它通过给出它应该如何工作的几个例子来验证我们想要测试的代码（在本例中为 Enum.sort/1）：

property_based_testing/sorting/test/example_based_sort_test.exs

```
```elixir
defmodule ExampleBasedSortTest do
  use ExUnit.Case

  test "Enum.sort/1 sorts lists" do
    assert Enum.sort([]) == []
    assert Enum.sort([1, 2, 3]) == [1, 2, 3]
    assert Enum.sort([2, 1, 3]) == [1, 2, 3]
  end
end
```
```

在这个例子中，我们验证对空列表进行排序返回空列表，对有序列表进行排序使其保持不变，以及对未排序列表进行排序返回该列表的排序版本。我们选择了三个我们认为将涵盖 Enum.sort/1 的所有可能输入的代表性样本的例子。这可行，但你可以看到我们还可以测试更多输入，例如负数、包含重复项的列表等等。

有时，我们尝试测试更多输入，同时通过提取输入和相应的预期输出然后在这些输入和输出上运行测试来简化测试：


```
property_based_testing/sorting/test/tabular_sort_test.exs

defmodule TabularSortTest do
  use ExUnit.Case

  test "Enum.sort/1 sorts lists" do
    inputs_and_outputs = [
      {[], []},
      {[1, 2, 3], [1, 2, 3]},
      {[2, 1, 3], [1, 2, 3]},
      {[2, 1, 2], [1, 2, 2]},
      {[0, -1, -2], [-2, -1, 0]}
    ]

    for {input, expected_output} <- inputs_and_outputs do
      assert Enum.sort(input) == expected_output
    end
  end
end
```

使用这种方法时，测试看起来像 *断言表*。断言表是输入和输出的表格，测试断言对表格中每一行的输入运行代码后得到的结果与该行相同的输出。因此，基于示例的测试的另一种名称是 *表格测试*。

这种测试有很多好处。首先，这类测试很容易编写，因为我们知道代码的工作方式，而且通常很容易为正在测试的代码想出输入。另一个好处是，由于我们指定了所有输入，因此可以选择测试我们怀疑可能对代码有问题的边界情况。在我们这个简单的排序示例中，我们知道空列表是一个边界情况，因为它是一个特殊的列表，所以每次运行测试时都可以直接对其进行测试。

然而，这些测试也有一些缺点。每次测试运行都测试相同的已知输入意味着很难发现未知的边界情况，因为它们就是未知的。同时，很难发现代码不支持或应该支持的输入，因为编写测试示例的是我们自己。让我们看看如何改善这种情况。

## 引入随机性和基于属性的测试

我们可以通过在测试中引入一点混乱来解决基于示例的测试所面临的一些问题。使用随机性生成输入将使我们能够对代码测试更广泛的输入，并可能创建触发边缘情况的输入。

在我们的排序示例中，我们真正想要测试的是 Enum.sort/1 的输出是输入的已排序版本。对于任何随机输入，我们可以想到输出始终会保留的几个属性。例如，输出列表始终与输入列表具有相同的长度。另一个属性是输出列表始终是有序的，我们可以通过检查每个元素是否小于或等于下一个元素以相当直接的方式检查这一点。现在我们已经考虑了这些属性，我们可以更改测试，以便生成随机列表并在代码的输出上测试这些属性，而不是检查输出是什么。让我们看看如何做到这一点：

```
property_based_testing/sorting/test/randomized_sort_test.exs

```elixir
1: defmodule RandomizedSortTest do
  use ExUnit.Case

  test "Enum.sort/1 sorts lists" do
5:   for _ <- 1..10 do
      random_list = random_list()
      sorted_list = Enum.sort(random_list)

      assert length(random_list) == length(sorted_list)
      assert sorted?(sorted_list)
10:
    end
  end
```
```

```
- defp random_list do
  Stream.repeatedly(fn -> Enum.random(-100..100) end)
15:
- |> Enum.take(_length = Enum.random(0..10))
- end
- 
- defp sorted?([first, second | rest]),
  do: first <= second and sorted?([second | rest])
20:
- 
- defp sorted?(_other), do: true
- end
```

random_list/0 函数创建一个介于 -100 和 100 之间的无限随机数流，然后使用 Enum.take/2 从流中选取随机数量的元素。我们从流中选取的元素数量是随机列表的长度，我们将其保持在 0 到 10 个元素之间。sorted?/1 函数检查列表的前两个元素是否已排序，然后递归检查列表的其余部分，直到到达空列表或单元素列表，这些列表始终是有序的。在第 9 行，我们检查第一个属性，即排序后的列表与输入列表具有相同数量的元素。在第 10 行，我们检查第二个属性，即排序后的列表是有序的。

这种测试方法有几个好处。最明显的一个是它有可能比基于示例的测试测试更多输入。在我们的示例中，如果我们想将测试列表的数量更改为一百或一千，我们只需更改第 5 行范围的右端即可。然而，除非输入各不相同，否则在许多输入上进行测试的实用性是有限的。

## 随机性的作用

这就是随机性发挥作用的地方。通过随机生成大量输入，我们希望覆盖代码可能输入的相当一部分，同时覆盖良好的输入多样性。

本质上，我们想要一组代表 *输入空间* 的良好输入样本，即所有可能输入的集合。在我们的示例中，我们仍然只覆盖了输入空间的一小部分（所有数字列表），但覆盖整个输入空间通常是不可行的。随机生成为我们提供了一个不错的折中方案，尤其是考虑到每次运行测试时可能会生成不同的列表。生成随机元素也有助于我们发现未预料到的潜在边界情况。

你可能会问自己随机性有多少是 *足够* 的，也就是说，你需要生成多少输入或需要运行这些测试多少次才能确信它们覆盖了足够的输入空间。在许多情况下，输入空间是无限的或太大而无法覆盖，但只有你根据具体用例才能知道要将其推向何种程度。

我们为 Enum.sort/1 编写的测试是称为 *基于属性的测试* 的一种测试的示例。它们之所以被称为这样，是因为我们用来设计这种测试的方法：我们思考 *属性*，只要输入有效，无论我们提供什么输入，这些属性都成立。

基于属性的测试的好处并不止于我们刚刚讨论的内容。提出有效输入和属性是基于属性测试的重要组成部分，但它也是一个有用的设计工具。如果你必须用清晰的术语写下代码的有效输入是什么，你可能会最终扩大或缩小有效输入的空间。相反，提出属性会迫使你思考代码无论提供什么特定输入都应该做什么，这可能有助于代码的设计或实现。在列表排序示例中，功能很简单，因此很难看到基于属性测试的设计优势；但在更复杂的上下文中，思考这些事情可能是有用的。

基于属性的测试很少像我们在示例中那样手工完成，因为有大量框架（适用于各种编程语言）可促进基于属性测试的实现。通常，基于属性的测试框架提供强大的数据生成方式以及验证生成数据属性的基础设施。还有一个重要的特性使使用基于属性的测试框架明显优于自行构建基于随机性的测试：框架在出现故障时会 *简化* 随机生成的输入，并且它们提供的错误消息往往比像我们这样手工编写随机数据测试更容易理解和解决。

对于 Elixir，我们从现在开始要使用的基于属性的测试框架称为 stream_data.[49]

## 为什么使用 stream_data？

Elixir 和 Erlang 生态系统通过成熟的库对基于属性的测试有良好的支持，例如 Quviq 的 QuickCheck 和 PropEr for Erlang,[50][51]，以及 PropCheck,[52] Quixir,[53] 和 stream_data for Elixir。

然而，我们偏向于 stream_data，因为 Andrea 编写了原始库，这意味着我们很熟悉它并且感到舒适。无论如何，我们将要说明的基于属性的测试概念与所有库都适用。

我们觉得提供一些关于 stream_data 最初为何被创建的背景，即使当时已有其他基于属性的测试框架，也可能对读者有所帮助。一个原因是最初的计划是将 stream_data 包含在 Elixir 标准库中，这意味着必须从头编写一些东西，以确保许可不是问题，并且 Elixir 核心团队能够维护代码。团队后来意识到 stream_data 作为库已经足够好用，因此它最终没有进入 Elixir 本身。另一个原因是所有现有的基于属性的测试框架只会在基于属性的测试的上下文中生成随机数据，而没有利用 Elixir 流使数据生成成为通用工具。




## 介绍 stream_data

stream_data 是一个用于 Elixir 的基于属性的测试框架。它提供了两个主要功能：数据生成以及编写和运行属性的框架。库的数据生成方面可在基于属性的测试之外作为独立功能使用，但它是整个框架的核心，在编写属性时也被广泛使用。

跟随接下来的几节内容，请使用 $ mix new sorting 创建一个新的 Mix 项目，然后在 mix.exs 文件中添加 :stream_data 作为依赖：

```
property_based_testing/sorting/mix.exs

defp deps do
  [{:stream_data, ">= 0.0.0", only: [:dev, :test]}]
end
```

现在，运行 $ mix deps.get 来获取依赖。如代码所示，我们只在 :test 环境中添加了 :stream_data，因为我们只会在测试时使用该库。

在深入框架之前，让我们重写之前手工编写的 RandomizedSortTest 测试，使用 stream_data 提供的工具：

```
property_based_testing/sorting/test/randomized_sort_stream_data_test.exs

1: defmodule FirstStreamDataPropertySortTest do
   use ExUnit.Case
   use ExUnitProperties

5:   property "Enum.sort/1 sorts lists" do
     check all list <- list_of(integer()) do
       sorted_list = Enum.sort(list)
```

```
-   assert length(list) == length(sorted_list)
    assert sorted?(sorted_list)
10:
-   end
-   end
- 
-   defp sorted?([first, second | rest]),
    do: first <= second and sorted?([second | rest])
15:
- 
-   defp sorted?(_other), do: true
-   end
```

不用担心这个测试中出现的新的内容。我们将在本章中全部介绍。这里的目标是向你展示 stream_data 的样子。现在，在你添加此文件所在的项目中运行 mix test，看看漂亮的绿色点。

事实证明，测试的底层结构与 RandomizedSortTest 非常相似。我们不再使用 test 宏来定义测试，而是使用 property（第 5 行）。然后我们在第 6 行使用一个新的构造 check all。这取代了我们之前使用的 for 推导式。在同一行，我们有 list <- list_of(integer())。这正是基于属性框架最重要的特性之一：数据生成器。在这里，stream_data 会为你处理随机数据的生成（我们稍后会看到其有趣的特性）。现在我们对 stream_data 测试的样子有了一个概念，让我们继续更详细地剖析其组成部分。

在接下来的章节中，我们将从 stream_data 的数据生成方面开始探索，然后再转向设计和运行属性。跟随操作，请运行 iex -S mix 从包含 stream_data 作为依赖的项目根目录启动一个 IEx 会话。

你可能会想知道为什么我们不会在之前章节中开发的应用之一（例如 Soggy Waffle）上说明这些概念。原因是我们必须以奇怪的方式扭曲那些应用才能有效地展示这些想法。相反，我们决定使用简单、小巧且自包含的示例，以便专注于基于属性的测试概念和工具。

## 数据生成

stream_data 的设计目标之一是提供一套数据生成工具，这些工具也可以在基于属性的测试之外使用。例如，生成随机数据在为数据库填充假数据时很有用。

数据生成的核心是生成器。生成器是一个包含 stream_data 用于生成数据的逻辑的数据结构。本质上，生成器就像一个我们可以调用来生成随机项的函数。让我们从一个简单的生成器实例 StreamData.integer/0 开始。这个生成器产生随机整数。由于它们实现了 Enumerable 协议，我们可以将所有 stream_data 生成器用作 Elixir 流。

```
iex> StreamData.integer() |> Enum.take(5)
[-1, 2, -3, 0, 2]
```

stream_data 生成器是无限的随机数据流，因此在这个示例中我们只需要使用 Enum.take/2 从流中取出几项。如果我们调用 Enum.to_list/1 并将生成器作为参数传递，我们将永远等待下去。

stream_data 配备了一些用于简单数据类型的生成器，例如整数或布尔值的生成器，以及大量组合生成器的方法。例如，没有内置的非负整数（正整数加零）生成器，但我们可以使用 StreamData.map/2 将 abs/1 函数映射到 StreamData.integer/0 生成器上来轻松构建一个。这与 Enum.map/2 将函数映射到可枚举对象的方式类似。

```
non_negative_integer = StreamData.map(StreamData.integer(), &abs/1)
```

为什么我们使用 StreamData.map/2 而不是 Stream.map/2，既然生成器是 Elixir 流？Stream.map/2 在生成数据用于基于属性的测试之外的目的时是可行的，例如为数据库填充随机数据：

```
non_negative_integer = Stream.map(StreamData.integer(), &abs/1)
```

通常，所有操作流的函数（例如 Enum 和 Stream 标准库模块中的许多函数）都适用于 stream_data 生成器。然而，你应该只在数据生成的上下文中使用这些函数，而不要在基于属性的测试中使用。在基于属性的测试中使用生成器时，使用 Stream 函数而不是 StreamData 函数将使生成器无法使用。在上面的示例中，Stream.map/2 将返回一个普通的非负整数流，而 StreamData.map/2 将返回一个非负整数流，同时还提供基于属性的测试的特定功能。我们稍后在讨论缩小生成数据时将更详细地了解这一点。

映射只是组合生成器的方式之一。还有过滤以及基于更简单的生成器创建更复杂数据类型的几种方法。一个例子是 StreamData.list_of/1，它接受一个生成器并返回一个新的生成器，该生成器生成由我们传递给它的生成器产生的元素的列表。例如，要生成整数列表，我们可以使用 StreamData.list_of(StreamData.integer()) 生成器。

```
iex> StreamData.list_of(StreamData.integer()) |> Enum.take(3)
[[1], [], [1, -2]]
```

## 组合生成器的基本工具：绑定

虽然有几种组合生成器的方法，如 StreamData.map/2，但了解 StreamData.bind/2 很重要。bind/2 是一个强大的函数，其他组合生成器的方式通常可以基于它构建。它是你需要组合两个生成器时可以使用的基本工具。bind/2 接受一个生成器和一个

函数：该函数将使用生成器产生的每个元素调用，并应返回一个新的生成器。

让我们用一个例子来说明。假设我们有一个生成随机电子邮件域（例如 gmail.com 或 icloud.com）的生成器。我们想要生成随机的电子邮件，其中包含随机的字母数字用户名，后跟由我们的域生成器产生的随机域：

```
property_based_testing/misc/random_email_generator.exs

```exs
1: domains = ["gmail.com", "yahoo.com", "icloud.com"]
2: random_domain_generator = StreamData.member_of(domains)
3: username_generator = StreamData.string(:alphanumeric, min_length: 1)
4:
5: random_email_generator =
6:   StreamData.bind(random_domain_generator, fn domain ->
7:     StreamData.map(username_generator, fn username ->
8:       "#{username}@#{domain}"
9:     end)
10:   end)
```
```

在前几行中，我们可以看到我们的随机域生成器，它使用 StreamData.member_of/1 从给定列表中随机选取元素。第 3 行是我们用户名生成器，它生成字母数字字符串。然后我们有 bind/2 调用。我们对随机域生成器产生的值进行绑定，该值被传递给我们提供给 bind/2 的函数。该函数应返回一个新的生成器。在第 9 行，我们从用户名生成器开始，并映射一个函数，该函数将 @ 加上域连接到用户名生成器产生的值。StreamData.map/2 调用的结果是一个生成器。当 random_email_generator 需要生成随机电子邮件时，它首先会创建一个随机域，然后使用传递给 bind/2 的匿名函数返回的生成器来生成随机电子邮件。



bind/2 很强大，因为它让我们可以通过组合更简单的生成器来创建复杂的生成器。我们将看到它在基于属性的测试的上下文中被大量使用。bind/2 在这种情况下也是必要的：否则我们将无法取两个生成器并将它们组合在一起。

接下来，让我们看看生成器如何通过生成大小来控制生成数据的复杂度。

## 生成大小

如果你从像 StreamData.integer/0 这样的生成器中取出几个元素，你会注意到这些整数很简单，也就是说它们是围绕零的较小整数。现在，尝试在丢弃大量元素后从 integer/0 中取出几个元素：

```
iex> StreamData.integer() |> Stream.drop(100) |> Enum.take(5)
[43, -96, 45, -17, 40]
```

如你所见，生成器产生的整数现在更复杂——也就是说，它们更大。这表明生成器产生的项越多，产生的项就越复杂。在从生成器流式传输项的情况下，这正是发生的情况。然而，其背后的机制更通用。为了让生成器生成一个项，我们需要向生成器传递一个大小，我们称之为生成大小。生成大小是一个正整数。生成器使用生成大小作为生成项复杂度的度量。一些生成器需要根据你的用例进行调整，以便它们生成“正确”的数据，因此为了有效地使用大多数基于属性的测试框架（包括 stream_data），花一点时间更好地理解生成大小是值得的。

让我们看看 integer/0 如何使用生成大小。当生成大小为 n 时，integer/0 会生成一个介于 -n 和 n 之间的整数。其他生成器可能会以不同的方式使用生成大小：例如，binary/1 使用它来确定生成二进制的长度。复合生成器（例如 list_of/1）会将生成大小传递给内部生成器。例如，list_of(integer()) 将使用生成大小来确定生成列表的长度以及列表中整数的“复杂度”。

你可能会想知道，当我们想要从生成器中生成一个元素时，生成大小是如何传递给生成器的。无法直接传递生成大小，因为生成对用户是隐藏的，要么是由 stream_data 在基于属性的测试中使用生成器时隐藏，要么是由 Enumerable 协议的实现隐藏。这是库本身的设计决策。

在使用生成器作为流时，Enumerable 协议的实现不允许在函数旁边传递额外信息（这将允许传递生成大小）。事实上，当生成器用作流时，它们以大小 1 开始生成，并且每次生成一个项时增加一。这意味着如果我们使用 Enum.take(generator, 5) 从生成器中取出五个元素，第五个元素将以生成大小 5 生成。虽然在生成元素时没有办法*直接*传递生成大小，但有几种间接方法可以操作它。这些方法有效地“包装”生成器，以便它可以携带生成大小信息。

stream_data 提供了一些函数来改变生成器对传递给它的生成大小的处理方式。

这些函数中最简单的是 StreamData.resize/2。该函数接受一个生成器和一个表示新生成大小的整数，并返回一个新的生成器。新生成器将始终使用给定的生成大小，忽略我们传递给它的生成大小。当你想要一个具有固定复杂度的生成器时，这很有用。

```
iex> StreamData.integer() |> StreamData.resize(50) |> Enum.take(5)
[28, 25, -44, 10, 41]
```

如果你想对生成大小应用一些修改，你需要使用 StreamData.scale/2 函数。它接受一个生成器和一个接受大小并返回新大小的函数，然后返回一个新的生成器。新生成器使用传递的函数返回的生成大小。（这有点啰嗦。）本质上，它以动态方式根据原始生成大小修改生成器的生成大小。

缩放在几种场景中特别有用。第一个场景是当你想要增加或减少生成大小的进展速度时。例如，对于复杂且深度嵌套的生成器，减少生成大小的增长可能是一个好主意。为此，我们可以使用平方根等数学函数来缩放生成大小：

```
iex> generator = StreamData.list_of(StreamData.string(:ascii))
iex> scaled_generator =
...> StreamData.scale(generator, fn size ->
...>   round(:math.sqrt(size))
...> end)
iex> scaled_generator |> Stream.drop(50) |> Enum.take(3)
[[""], ["#HdJ", "x>", "3y[/", "D", "FFe?"], ["", "", "+,"]
```

如你所见，即使在丢弃五十个元素之后，我们仍然生成包含小字符串的小列表。

缩放的另一个用例是对生成大小设置上限。

```
capped_integer =
StreamData.scale(StreamData.integer(), fn size -> min(size, 20) end)
```

在这个例子中，capped_integer 生成器将使用给定的生成大小直到上限，即 20，然后对于更大的生成大小我们将继续使用 20。毕竟，resize/2 只是 scale/2 的一个特定版本，其中传递给 scale/2 的函数忽略给定的生成大小并始终返回固定大小。

stream_data 提供的最后一个与大小相关的函数是 StreamData.sized/1。它仅使用生成大小构造一个生成器。如果我们想根据生成大小确定要使用的生成器，这个函数很有用。例如，我们可能想要一个生成器，它在给定的生成大小之前只生成整数，然后开始生成浮点数：

```elixir
### property_based_testing/misc/sized_generator.exs
StreamData.sized(fn size ->
  if size < 20 do
    StreamData.integer()
  else
    StreamData.one_of([StreamData.integer(), StreamData.float()])
  end
end)
```

了解生成大小的目的并了解我们提到的函数是很重要的，这样我们就可以为我们的用例生成更好的数据。生成大小在使用生成器作为流时以及在基于属性的测试的上下文中使用生成器时都会使用。

让我们做一个最后的实际例子，说明在现实世界中控制生成大小如何有用。我们将从之前讨论的 random_email_generator 生成器开始。在该生成器中，我们为电子邮件使用了一个固定的域名列表（例如 "gmail.com"）：

```elixir
### property_based_testing/misc/random_email_generator.exs
domains = ["gmail.com", "yahoo.com", "icloud.com"]
random_domain_generator = StreamData.member_of(domains)
```

我们本可以有一个“更智能”的域名生成器，它执行以下操作：

- 对于较小的生成大小，选择最常见的域名之一（有效地*以这些已知域名*开始生成），并且
- 在某个生成大小之后使用 `string/2` 生成器生成随机域名（通过缩放生成器使其保持较小）。

这个例子看起来可能有点复杂，但它与实际代码中一个好的电子邮件域名生成器的样子并不遥远。请注意，我们强制域名以 .com 结尾：这只是为了保持生成器的完整性，并避免必须使用 StreamData.bind/2 将随机域名与随机 TLD（例如 .com、.org、.net 等）组合：

```elixir
### property_based_testing/misc/random_email_domain_generator.exs
common_domains = ["gmail.com", "yahoo.com", "icloud.com"]
random_email_domain_generator =
  StreamData.sized(fn
    size when size <= 5 ->
      StreamData.member_of(common_domains)
    _size ->
      StreamData.string([?a..?z], min_length: 1)
      |> StreamData.scale(fn size -> trunc(:math.log(size)) end)
      |> StreamData.map(fn string -> string <> ".com" end)
  end)
```

现在我们熟悉了生成器，让我们看看 stream_data 提供的用于编写属性的工具。在下一节中，我们将首先使用 stream_data 库编写一些属性。然后我们将研究一些可以使编写属性变得更容易的设计模式。



## 编写属性

正如我们在初始示例中所见，基于属性的测试的一部分是生成数据，另一部分是提出属性。在本节和下一节中，我们将重点关注后者。

提出属性时要做的第一件事是弄清楚我们的代码所处理的合法输入的形状是什么。“Shape”不是技术术语或定义：我们用它来描述输入可能取的值的空间，例如所有字符串或所有整数列表。弄清楚合法输入的形状是在基于属性的测试的上下文中生成器发挥作用的地方。如果我们的代码使用整数列表，因此合法输入的形状是“lists of integers”，我们可以使用 StreamData.list_of(StreamData.integer()) 生成器来产生该形状的可能值的样本。你可以创建并组合生成器，使其输出符合代码接受的形状的有效数据。

弄清楚代码接受的合法输入后，我们需要提出代码在输入合法的前提下，无论输入如何都成立的属性。

一旦我们有了生成所需形状输入的生成器以及对所有这些输入都成立的属性，那么基于属性的测试框架的工作就是提供生成数据和验证属性的基础设施。stream_data 捆绑了工具和 DSL，让你能够做到这一点。你现在已经了解 stream_data 生成器了，缺少的部分就是使用这些生成器来产生输入并对这些输入测试属性。让我们先使用 stream_data 提供的工具重写 Enum.sort/1 函数的属性，以便看看 stream_data 在实际运行中的样子。然后我们再更详细地检查这段代码。

```
property_based_testing/sorting/test/first_property_sort_test.exs

1: defmodule FirstPropertySortTest do
  -   use ExUnit.Case
  -   use ExUnitProperties
  -
5:   property "Enum.sort/1 sorts lists" do
  -     check all list <- list_of(integer()) do
  -       sorted_list = Enum.sort(list)
  -
  -       assert length(list) == length(sorted_list)
          assert sorted?(sorted_list)
10:
  -
  -
  -     end
  -   end
  -
  -   defp sorted?([first, second | rest]),
        do: first <= second and sorted?([second | rest])
15:
  -
  -
  -   defp sorted?(_other), do: true
  - end
```

为了使用 stream_data 提供的基于属性的测试工具，我们首先需要在第 3 行使用 use ExUnitProperties 调用。这将导入所有生成器函数（如 list_of/1 或 integer/0）以及使 stream_data DSL 可用。DSL 由我们在第 6 行使用的 StreamData.check/2 宏组成。该宏类似于 for comprehensions 的语法，其中子句（<-）右侧有一个生成器，左侧有一个通用模式（在我们的例子中，该模式是一个简单的变量）。使用这种语法，stream_data 将负责从生成器生成一个元素，将其匹配到 <- 子句的左侧，然后运行 do/end 主体的内容。如果断言没有失败，stream_data 将重复该过程。

默认情况下，check all 会运行断言一百次，但可以通过多种方式进行配置。有时，你会有生成复杂值需要更长时间的生成器，或者你需要为单个属性使用多个生成器。在这些情况下，降低 check all 的运行次数可能会有所帮助。

运行我们的 SortTest case 与运行任何测试用例相同。如果我们在一个 Mix 项目中，我们将使用 mix test。输出的唯一区别是属性会被报告为属性而不是测试。

```
> mix test
..

1 property, 1 test, 0 failures
```

不用担心输出中的“1 test”部分。那是 mix new 在脚手架项目时生成的虚拟测试。

为了充分利用许多 stream_data 功能，了解 check all 宏很重要。让我们更仔细地看看它。

### The Check All Macro

check all 支持多个 <- 子句。它还支持简单的赋值（使用 =）以及用作过滤器的表达式，与 for 支持的功能类似。为了展示 check all 的更复杂用法，让我们写一个测试示例：首先生成一个列表，过滤掉空列表，从该列表中生成一个元素，最后检查 in 运算符对这两个生成的项始终返回 true：

```
property_based_testing/misc/check_all_clauses.exs

check all list <- list_of(term()),
    list != [],
    member <- member_of(list) do
assert member in list
end
```

结合这里的示例，可能更容易理解我们之前学到的 StreamData.bind/2 为什么如此重要。生成一个列表然后从该列表中生成一个随机元素本质上就是对 bind/2 的调用。

check all 还支持在 <- 子句之后传入的选项。你可以在 check all 的文档中找到完整的选项列表，但其中一个重要选项是 :max_runs。:max_runs 让你可以配置 check all 生成所有输入并检查属性成立的最大次数。默认情况下，check all 运行一百次，但对于较慢的属性，降低运行次数或为了覆盖更多输入空间而增加运行次数可能是有用的。以下是如何为较慢的属性减少最大运行次数的示例：

```
### This is executed only 10 times.
check all value <- some_generator(), max_runs: 10 do
  slow_code(value)
end
```

### The Reason for “Check All”

关于 stream_data 的一个常见问题是为什么 check all 写成两个词而不是一个词（check_all）。原因是 check all 需要欺骗 Elixir 编译器才能工作。Elixir 支持宏，它们是在编译时执行的函数，接收代码作为抽象语法树（AST，代码作为数据的表示）并返回 AST。由于函数和宏的元数必须固定，Elixir 不允许可变数量参数的宏。<- 子句的 AST 是 :<- 运算符对左侧和右侧应用的 AST。这意味着每个 <- 子句都将成为可能的 check_all 宏的一个参数。然而，我们希望子句的数量是可变的，这样就不会工作。我们使用的技巧是编译器会解析代码，然后使用对 all(clause1, ...) 的调用的 AST 调用 check/2 宏。all 函数不存在，但在解析时还不知道这一点，因此 AST 会愉快地传递给 check/2，后者可以按需操作它。for 和 with，Elixir 中使用相同语法的两个内置结构，可以避免这个技巧，因为它们是特殊形式，在语言级别直接处理。有了这个技巧，我们就可以拥有看起来像 for 和 with 的语法，而无需更改 Elixir 的编译器或运行时。很棒，对吧？

stream_data 还提供了与 check all 类似的语法来创建生成器。我们将在下一节中查看它。

### The Gen All Macro

通过使用 bind/2 之类的函数组合其他生成器来编写新生成器是可以的，但对于复杂的生成器，语法可能会变得有点复杂。因此，stream_data 提供了 gen all 宏。该宏看起来与 check all 完全相同，但它创建的是一个生成器而不是进行检查。生成器产生的值是传递给 gen all 的块返回的任何内容。让我们看一个如何使用该宏生成电子邮件的示例，从随机的字母数字用户名和从域名列表中随机选择的域名开始。

```
property_based_testing/misc/email_generator_gen_all.exs

domains = ["gmail.com", "yahoo.com", "icloud.com", "hotmail.com"]

email_generator =
  gen all username <- string(:alphanumeric),
    domain <- member_of(domains) do
  username <> "@" <> domain
end
```

正如你所见，<- 子句用于从生成器中取出值，就像在 check all 中一样。生成的值是 do/end 块的内容，在我们的例子中是构成电子邮件的连接字符串。



check all 支持 check all 支持的所有子句，例如过滤子句和赋值（=）子句。

现在我们已经编写了一些属性，让我们看看基于属性的测试框架如何处理属性中的失败。

## 缩小

在本节中，我们将研究当基于属性的测试失败时会发生什么。正如我们在本章开头提到的，基于属性的测试框架（如 stream_data）在这些情况下会通过缩小导致测试失败的输入并提出一个仍然导致失败的更简单的输入来帮助你。缩小在基于属性测试的有用性中发挥着基础性作用，了解其工作原理对于充分利用测试框架非常重要。

### 属性失败是什么样的

我们已经看到基于属性的测试如何帮助我们编写健壮的测试，部分原因是它覆盖了更大的输入空间来提供给我们的代码。然而，我们还没有讨论当测试在代码中发现错误时会发生什么。为了了解在这种情况下会发生什么，让我们设计一个我们知道不会很好工作的排序函数。最简单的一个想法是恒等函数，即一个函数接收一个参数并返回该参数不变。这是一个糟糕的排序函数，因为，嗯，它对输入列表什么也没做。然而，这个“sort”函数仍然至少满足我们一直在测试的两个属性之一：已排序列表的长度与输入列表的长度相同。这是有道理的，因为它就是同一个列表。然而，恒等函数肯定不满足输出列表已排序的属性。让我们将其写成一个属性并看看失败时会发生什么：

```
property_based_testing/sorting/test/bad_sort_test.exs

1: defmodule BadSortTest do
  - use ExUnit.Case
  - use ExUnitProperties
  -
5: property "bad_sort/1 sorts lists" do
check all list <- list_of(integer()) do
  IO.inspect(list)
  sorted_list = bad_sort(list)

  assert length(list) == length(sorted_list)

  assert sorted?(sorted_list)
end
end

defp bad_sort(list), do: list

defp sorted?([first, second | rest]),
  do: first <= second and sorted?([second | rest])

defp sorted?(_other), do: true

end
```

当我们运行此测试时，它按预期失败，这要归功于我们在第 15 行实现排序函数的方式（作为恒等函数）。让我们看看属性失败是什么样的：

```
> mix test

1) property bad_sort/1 sorts lists (BadSortTest)
test/bad_sort_test.exs:5

Failed with generated values (after 3 successful runs):

* Clause:  list <- list_of(integer())
Generated: [1, 0]

Expected truthy, got false
code: assert sorted?(sorted_list)
arguments:

### 1
[1, 0]
```

stacktrace: stacktrace

Finished in 0.04 seconds
1 property, 1 failure
```

这看起来与我们通常的 ExUnit 测试有点不同。stream_data 会在常规 ExUnit 错误中添加高亮行中的信息。本节包含有关生成数据的的信息。第一行告诉我们 stream_data 在找到失败之前生成了多少个项。在这种情况下，它在找到会导致失败的一个之前成功生成了三个列表。之后，stream_data 会报告 check all 中每个子句生成的数值（我们这里只有一个子句）。当有多个子句时，这更有用，因为它有助于追溯生成了哪些数据片段。

我们预计我们的糟糕排序函数会对任何未排序的列表失败。这正是发生的情况：一旦我们得到一个简单的未排序列表 [1, 0]，测试就失败了。再想一想，列表导致失败是该特定失败可能的最简单列表，这不是有点可疑吗？通过再次运行属性可以证实这种怀疑。如果我们多次运行属性，[1, 0] 大多数时候都会是导致失败的列表。替代结果可能看起来相似：例如，[0, -1] 或 [2, 1]。为了更好地了解正在发生的事情，我们可以在 check all 的 do/end 块的第一行立即打印生成的列表。

```
> mix test

[]
[-1]
[1, 2]
[1, 1, 0]
» [1, 0]
» [0]
» [1]
```

其余测试结果

让我们弄清楚这些生成的列表。第一个是 []，它是一个已排序的列表，所以我们的测试通过。[-1] 和 [1, 2] 也是如此。然后，我们的生成器吐出 [1, 1, 0]，这使得我们的测试失败。为什么这不是测试失败中显示的列表？

原因在于像 stream_data 这样的基于属性的测试框架会对导致属性失败的项执行一个称为缩小的过程。缩小包括简化生成的项，使其仍然导致测试失败，但使其更易于理解。在我们的示例中，我们可以在高亮行中看到缩小过程的实际效果。[1, 1, 0] 首先通过删除第一个元素减少为 [1, 0]。[1, 0] 仍然失败，所以 stream_data 尝试通过取出更多元素来进一步缩小它。它尝试 [0] 和 [1]，但这些是已排序的列表，所以我们的属性成立，我们必须退回到 [1, 0]。从 stream_data 的角度来看，0 和 1 是最简单的整数，所以框架不会尝试简化它们的值。这意味着导致失败的最简单列表已被找到。

在如此简单的示例中，很难看出缩小的价值。然而，想象一下你正在生成复杂的数据并将其提供给复杂的代码。在这些情况下，缩小是基础性的，因为它可以使更容易理解生成的哪些方面导致了失败。然而，重要的是要理解缩小过程是启发式的，这意味着它并不总是能找到导致失败的最简单的项。探索更简单数据的许多可能性需要时间，所以 stream_data 会继续尝试几种缩小生成数据的方法。然而，之后（默认情况下是一百次尝试），它会停止并报告它能想到的最简单的项。这就是为什么同样的测试可能会得出导致失败的最简单列表是 [0, -1]、[2, 1] 或类似的结论，以及为什么你在运行测试时可能会看到略有不同的结果。

## 缩小与生成器

还记得我们谈论数据生成器时说过，即使它们是 Elixir 流，你也应该使用 StreamData 函数来操作它们吗？好吧，原因就是缩小——stream_data 生成器会生成携带自身缩小逻辑的项。例如，integer/0 生成器会生成知道如何“缩小自身”的整数。当你通过 StreamData 函数组合生成器时，组合也会传递到缩小逻辑中。这是在一开始使用基于属性的测试框架而不是自己构建基于随机性的测试的最有价值的原因之一。让我们看一个快速示例。让我们拿我们的朋友 bind/2 并用它生成一个包含两个元素的元组：一个列表和该列表中的一个随机元素：

```
bind(list_of(term()), fn list ->
  tuple({constant(list), member_of(list)})
end)
```

我们使用 constant/1 将一个简单项转换为生成器，并使用 tuple/1 返回一个元组生成器。在缩小过程中，首先缩小内部元组生成器生成的项，但随后由 list_of(term()) 生成的初始列表会被缩小并用于在匿名函数中生成新的元组生成器。这种行为可能看起来很复杂，但大多数时候用户不需要关心在组合生成器时幕后发生了什么。即使如此，stream_data 也记录了每个生成器如何缩小以及每个组合函数如何影响缩小。

## 设计属性的策略

到目前为止，我们只在简单的场景中查看了基于属性的测试，在这些场景中，很容易想出能让我们对代码充满信心的良好属性。然而，在不太直接的情况下，基于属性测试的最大挑战之一是想出好的属性。就在本章开头，我们看到了 Elixir 团队如何在真实场景中使用基于属性的测试，以及他们如何想出一个属性来帮助测试代码 *and* 驱动其行为。

在本节中，我们将讨论如何更轻松地为自己的代码设计属性。我们将首先向你展示一些你可以识别并应用于代码的设计模式。然后我们将快速介绍在想出属性时常见的一些陷阱。

### 设计模式

让我们深入研究一些可以帮助属性设计的模式。这些模式可以帮助你完成设计高效、准确且有用的属性来测试代码的困难任务。我们将讨论的示例和想法并不特定于 stream_data，因为它们适用于基于属性的测试的一般情况，无论框架如何。

### 循环代码

只要你有两个代码片段，其中第一个片段做某事而第二个片段“撤销”它，就可以应用此模式。这就是名称 *circular code* 的由来。这种代码的常见示例是编码和解码。以 JSON 协议为例。如果你编写 JSON 编码器/解码器，它可能会提供一个函数将项编码为 JSON，以及一个函数将项从 JSON 解码。当你发现循环代码时，通常很简单

为其设计一个属性来验证代码确实是循环的。
让我们看一个这个虚构的 JSON 库的示例：

```
property_based_testing/misc/circular_property.exs

property "encoding + decoding is circular" do
  check all term <- term() do
    assert term == (term |> JSON.encode() |> JSON.decode())
  end
end
```

这个属性可以检测编码丢失信息的情况，导致解码结果与编码时不同。

## Oracle Model

大部分时间我们倾向于从头编写代码，但有时我们需要重写现有功能。例如，有时需要在不同语言中重写某些内容，或重写代码以提高性能。在这些场景中，我们可以将现有系统称为 oracle。当我们拥有一个 oracle 并编写具有与 oracle 相同功能的系统时，我们可以通过测试我们的系统行为是否与 oracle 一致来利用基于属性的测试。

让我们看一个具体的例子。想象我们想编写本章中一直测试的用于排序列表的函数。Erlang 已经提供了用于排序列表的函数，:lists.sort/1，但我们想用纯 Elixir 编写实现。我们选择使用快速排序算法。让我们将此算法写在 ListSort.quicksort/1 函数中：

```
property_based_testing/sorting/lib/list_sort.ex

defmodule ListSort do
  def quicksort([head | rest]) do
    {smaller_elements, larger_elements} =
      Enum.split_with(rest, &(&1 <= head))

    quicksort(smaller_elements) ++ [head] ++ quicksort(larger_elements)
  end

  def quicksort([]), do: []
end
```

现在我们想检查我们的纯 Elixir quicksort 实现是否有效。有什么比对照一个已经存在且我们可以假设其工作正确的实现更好的方法呢，比如 :lists.sort/1？这个属性很简单，只需生成随机列表并断言通过我们的 quicksort 实现排序的结果与使用 :lists.sort/1 的结果相同即可。基本上，:lists.sort/1 就是我们的 oracle。让我们看看这个属性的实际效果。

```
property_based_testing/sorting/test/list_sort_test.exs

defmodule ListSortTest do
  use ExUnit.Case
  use ExUnitProperties

  property "quicksort/1 correctly sorts lists" do
    check all list <- list_of(term()) do
      assert ListSort.quicksort(list) == :lists.sort(list)
    end
  end
end
```

在我们的简单示例中，oracle 只是一个小的纯函数，但这种模式即使在 oracle 是一个完整系统时也很有用。

## Smoke Testing

冒烟测试指的是测试代码不会以意外的方式行为，而不是测试代码的确切行为的做法。其思想是运行仅验证没有严重损坏的测试。例如，我们的列表排序函数应该始终返回一个列表，或者在参数不是列表的情况下以 FunctionClauseError 失败，但它绝不应该以任何其他错误失败或返回
除列表之外的任何内容。如果出现这种情况，就意味着出了很严重的问题。列表排序示例过于简单，无法理解冒烟测试的好处。但当被测系统复杂时，冒烟测试可以显著更快地编写和运行，从而提供很大的价值。当为代码生成精确数据很困难且很难找到准确的属性来确保代码正确工作时，基于属性的测试仍然可以帮助进行冒烟测试。让我们看看我们的列表排序示例：

```
property_based_testing/sorting/test/list_sort_smoke_test.exs

defmodule ListSortSmokeTest do
  use ExUnit.Case
  use ExUnitProperties

  property "quicksort/1 returns a list or fails with FunctionClauseError" do
    check all term <- term() do
      try do
        ListSort.quicksort(term)
      rescue
        FunctionClauseError ->
          :ok

        other ->
          raise "raised unexpected exception: #{inspect(other)}!"
      else
        term ->
          assert is_list(term)
      end
    end
  end
end
```

正如我们提到的，这个例子是人为构造的，因为我们看到为测试列表排序函数想出属性很容易。然而，当被测系统变得更复杂时，这种模式变得有用。例如，让我们想象我们有一个 HTTP API。为了编写覆盖 API 交互的基于属性的测试，我们需要能够生成具有
有效路径、头信息和正文的有效 API 调用的复杂生成器。我们还需要能够提出这些调用响应的属性。经常很难做到这一点。然而，我们知道按设计我们的 API 可以在一切顺利时返回状态码 200 的响应，如果请求有错误则返回 400，如果 URL 未找到则返回 404。

在不知道系统其他任何信息的情况下，我们可以编写一个属性来生成随机的 HTTP 请求（具有随机的路径、头信息和正文），将其发送到我们的 API，然后断言 API 只返回其中一个预期的代码。我们不关心 API 返回的响应是否是对我们发送的请求的正确响应；我们只关心 API 不会因意外情况而崩溃。我们只是断言没有严重损坏。让我们看看这个属性在伪代码中可能是什么样子：

```
property_based_testing/misc/api_smoke_test.exs

property "API only returns a response with status of 200, 400, or 404" do
  host = "myapi.example.com"

  check all method <- http_method_generator(),
          path <- path_generator(),
          headers <- headers_generator(),
          body <- binary() do
    response = send_http_request(host, method, path, headers, body)
    assert response.status in [200, 400, 404]
  end
end
```

这种模式功能强大。它使用少量代码测试系统的大量输入，有机会找到使系统崩溃的输入，并且不需要我们提出好的属性。

## Avoiding Reimplementing Your Logic to Test It

对于基于属性测试（以及可能一般的测试）的初学者用户来说经常出现的一个问题是必须重新实现一段代码的逻辑才能对其进行测试的感觉。在我们的示例中，我们并没有太受这个问题困扰，但在我们必须实现一个 sorted?/1 函数来测试列表是否已排序时，我们可以看到这个问题在悄悄出现。那个函数比实际排序列表的函数简单。但我们可以想象，在为了检查我们的逻辑是否正确而必须在属性中重新实现大量原始逻辑的情况下。

有时重写一个工作实现可以是一种有用的技术。诀窍不是简单地重写相同的实现，而是专注于编写一个以牺牲其他东西（如性能）为代价让我们更有信心逻辑工作的实现。如果我们编写一个我们确信工作正确的低效版本代码，我们就可以将其用作我们试图测试的代码的 oracle。

## Are Properties Enough?

属性是否足以确保你的代码工作并正确行为？有时，是的。大多数时候，不是。属性验证代码的不变量，即无论输入如何都保持不变的代码属性。有时，错误的代码可以保持你正在测试的相同不变量。让我们看一个例子。想象我们有一个属性断言，当我们连接两个字符串时，我们可以使用 String.contains?/2 来验证连接后的字符串包含两个原始字符串：

```
property_based_testing/misc/property_and_example_test.exs

property "concatenation of a and b contains both" do
  check all left <- string(),
    right <- string(),
    concatenated = left <> right do
  assert String.contains?(concatenate, left)
```



```elixir
assert String.contains?(concatenate, right)
end
end
```

这是一个 String.contains?/2 应该满足的良好属性。然而，如果这是我们对 String.contains?/2 进行的唯一测试，那么我们就可以将 String.contains?/2 实现为一个始终返回 true 的函数。

```elixir
def contains?(_string, _substring), do: true
```

这种明显错误的实现仍然能通过我们的属性。因此，重要的是我们仍然要有单元测试或集成测试来实际验证我们的代码对某些给定输入是否按预期工作。与属性一起拥有测试对于测试代码的已知边界情况也很有用，因为那些有问题的输入可能并不总是被生成器产生。在 String.contains?/2 的例子中，我们可能会在属性旁边加上如下测试：

property_based_testing/misc/property_and_example_test.exs

```elixir
test "String.contains?/2 works on known inputs" do
  assert String.contains?("foobar", "foo")
  assert String.contains?("foobar", "bar")
  assert String.contains?("foobar", "ob")
  refute String.contains?("foobar", "baz")
end
```

这个测试会立即让总是返回 true 的虚拟实现失败。属性与基于示例的测试的结合非常强大：属性断言我们的代码保持不变式，而基于示例的测试验证我们的代码对少数已知（和边界）情况是否有效。这可以让我们对代码的正确性有很高的信心。

总的来说，属性测试通常不是可以替代基于示例测试的工具。相反，它是可以补充基于示例测试的工具。当有意义并且提出属性不是太困难时，属性测试可以让我们以简洁的方式测试大量输入和边界情况。

## 状态化属性测试

到目前为止，我们主要在测试纯粹、无状态的函数，即接收输入并返回输出的函数上下文中查看属性测试。然而，属性测试对于测试状态化系统也很有用。*状态化系统* 是指携带状态的系统。例如，数据库就是一个状态化系统。

在我们迄今为止的示例中，我们只使用属性测试来生成一些数据，然后将其馈送到一段代码并断言其结果。对于状态化系统，情况发生了变化：我们现在必须处理设置状态，并且只有在系统处于给定状态时才执行某些操作。让我们看看如何将属性测试用于这种情况。

## 对状态化系统的建模

我们知道如何通过属性测试框架生成随机数据。我们可以利用这一知识来生成可以对状态化系统发出的随机 *命令*。例如，如果我们的状态化系统是一个数据库，我们可以生成要对该系统发出的随机命令。然而，如果命令是随机的，我们如何断言它们对系统的影响呢？引入 *系统模型*。属性测试状态化系统的整个思想围绕着用模型来表示真实系统这一想法展开，该模型从我们感兴趣的角度表示该系统。一旦我们有了这个模型，我们就可以在真实系统和模型上执行生成的命令，然后验证命令的效果是否匹配。这听起来很复杂，所以让我们用一个例子来分解。

让我们想象我们编写了一个键值存储，我们可以在其中写入存储在唯一键下的值，并使用相应的键检索这些值。

```elixir
iex> kv_store = KVStore.new()
iex> KVStore.set(kv_store, "key1", "some value")
iex> KVStore.get(kv_store, "key1")
"some value"
iex> KVStore.delete(kv_store, "key1")
iex> KVStore.get(kv_store, "key1")
nil
```

这是一个状态化系统，其状态是使用 KVStore.new/0 创建的键值存储中存储的键值对集合。我们可以测试的关于该系统的一件事是，检索现有键始终返回为该键设置的最后一个值，或者如果最后一次操作是删除该值，则返回空值，无论我们设置或删除该键多少次。我们如何建模我们的键值存储以测试此属性？我们可以从一个简单的两元素元组 {key, value} 开始，它存储我们感兴趣的键及其当前值（如果键尚未设置，则为 nil）。让我们使用伪代码来看看如果我们已经生成了一个名为 random_key 的随机键，如何生成一个命令：

property_based_testing/misc/stateful_testing.exs

```elixir
def command do
  one_of([
    command(:set, [random_key, term()]),
    command(:delete, [random_key])
  ])
end
```

对于可以在状态化系统上发出的每种可能的命令，我们需要定义它对模型的影响。让我们继续使用伪代码：

property_based_testing/misc/stateful_testing.exs

```elixir
def set({key, _old_value}, key, new_value) do
  {key, new_value}
end
```

```elixir
def delete({key, _value}, key) do
  {key, nil}
end
```

现在我们还需要定义一个 get 命令来检索模型中键的当前值：

property_based_testing/misc/stateful_testing.exs

```elixir
def get({key, value}, key) do
  value
end
```

我们已在模型中反映了我们的命令。现在是有趣的部分。我们生成一个随机命令列表。然后，我们一次一个地在模型和真实系统上执行这些命令。最后，我们验证模型中存储的值是否与状态化系统中该键下存储的值相同。

我们之所以在这些示例中保持通用并且不展示可工作的代码，是因为 stream_data 仍然没有提供用于处理状态化测试的工具（即使未来可能会提供）。

## 状态化属性测试的好处

即使 stream_data 尚未支持状态化属性测试，我们仍然想提到它并介绍其背后的基本概念，因为它是一个强大且有用的工具。我们在本章中讨论的属性测试的所有好处同样适用于状态化属性测试。

状态化属性测试的一个特别有用的行为是，我们生成的随机命令列表在失败时会进行 *shrunk*。这意味着当属性失败时，框架会向我们展示一个由导致失败的小输入组成的小命令列表。这可能非常宝贵。谷歌的 leveldb 就是一个很好的证明，其中正如 Joseph Wayne Norton 的幻灯片[54]中所提到的，属性测试发现了 17 次调用的序列然后是 31 次调用的序列，这些序列会在数据库中生成幽灵键。那些是 *shrunk* 的命令序列！重现这些错误对人类来说将是一场噩梦。

## 总结

我们看到了属性测试如何成为一个强大的工具，可以通过对随机生成的输入进行测试来加强你的测试套件并提高对代码的信心。你学习了如何使用 stream_data 库进行数据生成以及属性测试。然后，我们查看了一些在设计属性时可能有帮助的模式。最后，我们快速浏览了状态化属性测试。需要记住的重要一点是，属性测试不是灵丹妙药，它不能取代其他类型的测试，但它可以成为测试套件的绝佳补充。

这是本书的最后一章。你做到了！你现在已经成为 Elixir 代码的熟练测试者。我们希望你能对所学的东西感到愉快（或至少富有成效）。祝测试愉快！

## 脚注

- [47] https://github.com/whatyouhide/stream_data
- [48] https://github.com/elixir-lang/elixir/pull/6559
- [49] https://github.com/whatyouhide/stream_data
- [50] http://www.quviq.com/products/erlang-quickcheck/
- [51] https://github.com/proper-testing/proper
- [52] https://github.com/alfert/propcheck
- [53] https://github.com/pragdave/quixir
- [54] http://htmlpreview.github.io/? https://raw.githubusercontent.com/strangeloop/lambdajam2013/master/slides/Norton-QuickCheck.html



