
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
# property_based_testing/misc/sized_generator.exs
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
# property_based_testing/misc/random_email_generator.exs
domains = ["gmail.com", "yahoo.com", "icloud.com"]
random_domain_generator = StreamData.member_of(domains)
```

我们本可以有一个“更智能”的域名生成器，它执行以下操作：

- 对于较小的生成大小，选择最常见的域名之一（有效地*以这些已知域名*开始生成），并且
- 在某个生成大小之后使用 `string/2` 生成器生成随机域名（通过缩放生成器使其保持较小）。

这个例子看起来可能有点复杂，但它与实际代码中一个好的电子邮件域名生成器的样子并不遥远。请注意，我们强制域名以 .com 结尾：这只是为了保持生成器的完整性，并避免必须使用 StreamData.bind/2 将随机域名与随机 TLD（例如 .com、.org、.net 等）组合：

```elixir
# property_based_testing/misc/random_email_domain_generator.exs
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
